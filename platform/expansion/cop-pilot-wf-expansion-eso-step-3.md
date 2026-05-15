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
' Step 3
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow follows up platform expansion step 2 "SIF client deployment on the new domain"

== Step 3: Register new compute cluster to host a COP-PILOT DO ==

DomainOwner -> "ESO\nPortal": Browse on domain clusters view
DomainOwner -> "ESO\nPortal": Add domain cluster
DomainOwner -> "ESO\nPortal": Fill-in domain cluster information\n(i.e., cluster config file)
DomainOwner -> "ESO\nPortal": Add cluster
"ESO\nPortal" -> "ESO\nBackend"
"ESO\nBackend" -> "Cluster\nController": Authenticate and connect
"Cluster\nController" -> "ESO\nBackend": Connected
"ESO\nBackend" -> "ESO\nPortal": Successful cluster onboarding
"ESO\nPortal" -> DomainOwner: Cluster visualized

note over "ESO\nPortal", "Cluster\nController": Go to step 4 "Order a new COP-PILOT DO for this domain"

```
