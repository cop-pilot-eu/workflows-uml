```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Developer" as Dev #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' ==========================================
' CI/CD Pipeline: Business Portal (BML)
' Deployment via Jenkins Automation Server
' ==========================================

== Trigger CI/CD Pipeline ==

Dev -> "Automation\nServer": Push code to main branch\nor trigger pipeline manually
"Automation\nServer" -> "Automation\nServer": Pipeline started

== Teardown existing deployment ==

"Automation\nServer" -> "BMP\nFrontend": docker compose down (remove old containers)
"BMP\nFrontend" -> "Automation\nServer": Old deployment removed

== Lint checks ==

"Automation\nServer" -> "Automation\nServer": Build builder image\n& run lint checks

== Build Docker image ==

"Automation\nServer" -> "Automation\nServer": docker compose build\n(production image)

== Functional & Smoke Tests ==

"Automation\nServer" -> "BMP\nFrontend": docker compose up (test instance on :3000)
"Automation\nServer" -> "BMP\nFrontend": Run func_test.sh smoke tests
"BMP\nFrontend" -> "Automation\nServer": Tests passed
"Automation\nServer" -> "BMP\nFrontend": docker compose down (test instance)

== Push image to Harbor Registry ==

"Automation\nServer" -> "Automation\nServer": docker login to Harbor\n(harbor-creds)
"Automation\nServer" -> "Automation\nServer": Push image (tagged + latest)

== Deploy Business Portal ==

"Automation\nServer" -> "Automation\nServer": docker compose pull\n(latest image from Harbor)
"Automation\nServer" -> "BMP\nFrontend": docker compose up -d (production)
"BMP\nFrontend" -> "Automation\nServer": Portal running on :3000

== Seed initial admin user ==

"Automation\nServer" -> "BMP\nFrontend": Wait for portal readiness
"BMP\nFrontend" -> "Automation\nServer": Portal ready
"Automation\nServer" -> "BMP\nFrontend": POST /api/auth/sign-up/email\n(admin credentials)
"BMP\nFrontend" -> "Automation\nServer": Admin user created (HTTP 200/201)

== Expose Portal publicly via SIF ==

"Automation\nServer" -> "SIF\nController": Create Ziti service\n(Business Portal on :3000)
"SIF\nController" -> "Automation\nServer": Service + policies created
"SIF\nController" -> "BMP\nFrontend": Route portal traffic\nvia SIF Frontdoor to :3000

note over "SIF\nController", "BMP\nFrontend"
  Business Portal publicly accessible
  via SIF Frontdoor at
  https://business.portal.cop-pilot.rid-intrasoft.eu/
end note

```
