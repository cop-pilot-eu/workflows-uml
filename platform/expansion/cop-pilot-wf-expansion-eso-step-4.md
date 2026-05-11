```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Domain\nOwner" as DomainOwner #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' General template providing COP-PILOT Cluster components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-cluster-generic.puml

' =====================
' Platform expansion to new private domain
' Step 4
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow follows up platform expansion step 3 "Register new compute cluster to host a COP-PILOT DO"

== Step 4: Order a new COP-PILOT DO for this domain ==

DomainOwner -> "ESO\nPortal": Browse on service Marketplace
DomainOwner -> "ESO\nPortal": Add relevant service specification into shopping cart
DomainOwner -> "ESO\nPortal": Configure service registry info, Kubernetes service ID
DomainOwner -> "ESO\nPortal": Place service order
"ESO\nPortal" -> "ESO\nBackend": Dispatch service order
"ESO\nBackend" -> "Cluster\nController": Deploy DO service package
"Cluster\nController" -> "DO\nBackend": Deploy
"Cluster\nController" -> "ESO\nBackend": Deployed
"ESO\nBackend" -> "ESO\nPortal": Successful DO provisioning
"ESO\nPortal" -> DomainOwner

note over "ESO\nPortal", "Cluster\nController": Go to step 5 "COP-PILOT DO resource discovery"

```
