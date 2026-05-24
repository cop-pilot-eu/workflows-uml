```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "User" as User #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' General template providing COP-PILOT Cluster components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-cluster-generic.puml

' =====================
' Service update flow
' for single-domain services
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow assumes a single-domain service is already deployed via the respective flow 

== Single-domain service update flow ==

User -> "ESO\nPortal": Browse on My Services view
User -> "ESO\nPortal": Click on service to update
User -> "ESO\nPortal": Modify customer facing service characteristic
User -> "ESO\nPortal": Place service update
"ESO\nPortal" -> "ESO\nBackend": Dispatch service update
"ESO\nBackend" -> "ESO\nBackend": Identify facilitator of service update
"ESO\nBackend" -> "DO\nBackend": Delegate service update
"DO\nBackend" -> "Cluster\nController": Update service
"Cluster\nController" -> "DO\nBackend": Updated
"DO\nBackend"-> "ESO\nBackend": Service update completed
"ESO\nBackend" -> "ESO\nPortal"
"ESO\nPortal" -> User: Updated service visualized on My services

```
