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

"Automation\nServer" -> "LLM-enhanced\nPortal": docker compose down (remove old containers)
"LLM-enhanced\nPortal" -> "Automation\nServer": Old deployment removed

== Lint checks ==

"Automation\nServer" -> "Automation\nServer": Build builder image\n& run lint checks

== Build Docker image ==

"Automation\nServer" -> "Automation\nServer": docker compose build\n(production image)

== Functional & Smoke Tests ==

"Automation\nServer" -> "LLM-enhanced\nPortal": docker compose up (test instance on :3000)
"Automation\nServer" -> "LLM-enhanced\nPortal": Run func_test.sh smoke tests
"LLM-enhanced\nPortal" -> "Automation\nServer": Tests passed
"Automation\nServer" -> "LLM-enhanced\nPortal": docker compose down (test instance)

== Push image to Harbor Registry ==

"Automation\nServer" -> "Automation\nServer": docker login to Harbor\n(harbor-creds)
"Automation\nServer" -> "Automation\nServer": Push image (tagged + latest)

== Deploy Business Portal ==

"Automation\nServer" -> "Automation\nServer": docker compose pull\n(latest image from Harbor)
"Automation\nServer" -> "LLM-enhanced\nPortal": docker compose up -d (production)
"LLM-enhanced\nPortal" -> "Automation\nServer": Portal running on :3000

== Seed initial admin user ==

"Automation\nServer" -> "LLM-enhanced\nPortal": Wait for portal readiness
"LLM-enhanced\nPortal" -> "Automation\nServer": Portal ready
"Automation\nServer" -> "LLM-enhanced\nPortal": POST /api/auth/sign-up/email\n(admin credentials)
"LLM-enhanced\nPortal" -> "Automation\nServer": Admin user created (HTTP 200/201)

== Expose Portal publicly via SIF ==

"Automation\nServer" -> "SIF\nController": Create Ziti service\n(Business Portal on :3000)
"SIF\nController" -> "Automation\nServer": Service + policies created
"SIF\nController" -> "LLM-enhanced\nPortal": Route portal traffic\nvia SIF Frontdoor to :3000

note over "SIF\nController", "LLM-enhanced\nPortal"
  Business Portal publicly accessible
  via SIF Frontdoor at
  https://business.portal.cop-pilot.rid-intrasoft.eu/
end note

```
