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
' Step 2
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow follows up platform expansion step 1 "Platform expansion request"

== Step 2: SIF client deployment on the new domain ==

DomainOwner -> "Cluster\nController": Deploy SIF Client with domain identity
"Cluster\nController" -> "SIF Client\nRemote": Deploy
"SIF Client\nRemote" -> "SIF\nController": Authenticate with domain identity
"SIF\nController" -> "SIF Client\nRemote": Successful authentication
"ESO\nBackend" -> "SIF\nController": Periodic pull of domain identities
"SIF\nController" -> "ESO\nBackend": Identities fetched
"ESO\nBackend" -> "ESO\nPortal": Domain identities update view
"ESO\nPortal" -> DomainOwner: New domain is connected

note over "ESO\nPortal", "Cluster\nController": Go to step 3 "Register new compute cluster to host a COP-PILOT DO"

```
