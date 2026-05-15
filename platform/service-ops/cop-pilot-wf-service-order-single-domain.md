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
' Service ordering flow
' for single-domain services
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow assumes a service specification for a given service is already designed\non a remote DO and imported to the ESO via the peering flow

note over "ESO\nPortal", "Cluster\nController": This flow also assumes a compute cluster is already available in the target domain\n and the cluster's API is exposed via the SIF

== Single-domain service ordering flow ==

User -> "ESO\nPortal": Browse on service Marketplace
User -> "ESO\nPortal": Add desired service specification into shopping cart
User -> "ESO\nPortal": Configure service info and\nhosting compute service ID
User -> "ESO\nPortal": Place service order
"ESO\nPortal" -> "ESO\nBackend": Dispatch service order
"ESO\nBackend" -> "ESO\nBackend": Identify facilitator of service order
"ESO\nBackend" -> "DO\nBackend": Delegate service order
"DO\nBackend" -> "Cluster\nController": Deploy service package
"Cluster\nController" -> "DO\nBackend": Deployed
"DO\nBackend"-> "ESO\nBackend": Service order completed
"ESO\nBackend" -> "ESO\nPortal"
"ESO\nPortal" -> User: Service visualized on My services

```
