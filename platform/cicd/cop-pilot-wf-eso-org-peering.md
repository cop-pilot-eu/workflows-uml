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
    COP_PILOT_BOX_WITH_LOGO_INNER("Domain\nOrchestrator (DO)")
        COP_PILOT_SERVICE("DO\nBackend")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
END_REMOTE_INFRA_BOX()

' =====================================================
' CI/CD Pipeline: HypO (ESO) TMF Organization
' Creation & Peering with OpenSlice (DO)
' =====================================================

== Trigger ESO Organization & Peering Pipeline ==

PltAdmin -> "Automation\nServer": Trigger pipeline manually\n(credentials, org metadata,\nOpenSlice URL)
"Automation\nServer" -> "Automation\nServer": Pipeline started

== Get OAuth Token ==

"Automation\nServer" -> "ESO\nBackend": POST /token\n(Resource Owner Password Grant)
"ESO\nBackend" -> "Automation\nServer": Access token issued

== Render Organization JSON ==

"Automation\nServer" -> "Automation\nServer": Load template, resolve geolocation,\ninject OpenSlice credentials\n→ organization_payload.json

== Create Organization in HypO (ESO) ==

"Automation\nServer" -> "ESO\nBackend": POST /tmf-api/party/v4/organization\n(organization payload)
"ESO\nBackend" -> "Automation\nServer": Organization created (HTTP 201)

== Start Peering Process ==

"Automation\nServer" -> "ESO\nBackend": POST /peering-api/peering\n(organization + empty specs)
"ESO\nBackend" -> "DO\nBackend": Fetch available service specs\nfrom OpenSlice
"DO\nBackend" -> "ESO\nBackend": Service specifications list
"ESO\nBackend" -> "Automation\nServer": Peering response\n(available service specs)

== Enrich & Add Peering Details ==

"Automation\nServer" -> "Automation\nServer": Filter specs, build enriched payload\n(service catalog + category)
"Automation\nServer" -> "ESO\nBackend": POST /peering-api/peering/add\n(enriched payload with specs & catalogs)
"ESO\nBackend" -> "Automation\nServer": Peering details added (HTTP 200)

== Cleanup ==

"Automation\nServer" -> "Automation\nServer": Remove temporary JSON artifacts

```
