```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Platform\nAdmin" as PltAdmin #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' Cluster X - Domain Y components (inlined)
REMOTE_INFRA_BOX("COP-PILOT Cluster X - Domain Y")
    COP_PILOT_BOX_WITH_LOGO_INNER("Data\nManagement (DM)")
        COP_PILOT_SERVICE("DM\nServer")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
END_REMOTE_INFRA_BOX()

' =====================================================
' CI/CD Pipeline: Orion-LD (NGSI-LD) Context Broker
' Deployment via Jenkins on Domain Orchestrator node
' =====================================================

== Trigger Orion-LD Deployment Pipeline ==

PltAdmin -> "Automation\nServer": Trigger pipeline manually\n(select NODE_LABEL, image tags, ports)
"Automation\nServer" -> "Automation\nServer": Pipeline started on target node

== Prepare workspace ==

"Automation\nServer" -> "Automation\nServer": Create deployment directory\n(orionld-stack/)

== Docker sanity check ==

"Automation\nServer" -> "Automation\nServer": Verify Docker & Compose\navailability

== Generate Docker Compose ==

"Automation\nServer" -> "Automation\nServer": Generate docker-compose.orionld.yml\n(Orion-LD + MongoDB, parameterized)

== Deploy Orion-LD + MongoDB ==

"Automation\nServer" -> "Automation\nServer": docker compose pull\n(Orion-LD & MongoDB images)
"Automation\nServer" -> "DM\nServer": docker compose up -d\n(Orion-LD on :1026, MongoDB on :27017)
"DM\nServer" -> "Automation\nServer": Stack running

== Smoke Test: Orion-LD /version ==

"Automation\nServer" -> "DM\nServer": GET /version (NGSI-LD API)
"DM\nServer" -> "Automation\nServer": Orion-LD version confirmed healthy

```
