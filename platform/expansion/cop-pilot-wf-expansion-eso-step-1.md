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
' Step 1
' =====================

note over "ESO\nPortal", "SIF\nController": This flow initiates platform expansion to a new private domain.\nIt assumes the ESO has already an account on the SIF platform.

== Step 1: Platform expansion request ==

DomainOwner -> "ESO\nPortal": Browse on platform expansion view
"ESO\nPortal" -> "ESO\nBackend": Create an identity for the new domain
"ESO\nBackend" -> "SIF\nController": Authenticate with ESO credentials
"SIF\nController" -> "ESO\nBackend": Successful authentication
"ESO\nBackend" -> "SIF\nController": Create domain identity
"ESO\nBackend" -> "ESO\nPortal": Domain identity created
"ESO\nPortal" -> DomainOwner: Domain identity checkout button
DomainOwner -> "ESO\nPortal": Download domain identity locally

note over "ESO\nPortal", "Cluster\nController": Go to step 2 "SIF client deployment on the new domain"

```
