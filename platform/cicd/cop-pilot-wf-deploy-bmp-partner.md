```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Partner\nAdmin" as PartnerAdmin #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

note over "BMP\nBackend"
  (Partner VM)
end note

' =====================================================
' CI/CD Pipeline: Business Portal (BML) Partner Deploy
' Pull from Harbor & deploy on Partner VM
' =====================================================

== Trigger Partner Deployment Pipeline ==

PartnerAdmin -> "Automation\nServer": Trigger pipeline manually\n(select AGENT_LABEL, DOCKER_TAG)
"Automation\nServer" -> "Automation\nServer": Pipeline started on partner VM

== Pull image from Harbor ==

"Automation\nServer" -> "Automation\nServer": docker login to Harbor\n(harbor-creds)
"Automation\nServer" -> "Automation\nServer": docker pull portal image\n(requested tag)

== Deploy Business Portal on Partner VM ==

"Automation\nServer" -> "BMP\nBackend": docker compose up -d\n(inject env variables)
"BMP\nBackend" -> "Automation\nServer": Portal running on :3000

== Seed initial admin user (optional) ==

"Automation\nServer" -> "BMP\nBackend": Wait for portal readiness
"BMP\nBackend" -> "Automation\nServer": Portal ready
"Automation\nServer" -> "BMP\nBackend": POST /api/auth/sign-up/email\n(partner admin credentials)
"BMP\nBackend" -> "Automation\nServer": Admin user created (HTTP 200/201)

```
