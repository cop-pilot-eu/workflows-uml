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
' Step 6
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow follows up platform expansion step 5 "COP-PILOT DO resource discovery, service design, and exposure"

== Step 6: Federation of new domain's Service Marketplace ==

DomainOwner -> "ESO\nPortal": Browse on Platform OSS view
DomainOwner -> "ESO\nPortal": Add new OSS
DomainOwner -> "ESO\nPortal": Configure OSS endpoint, credentials, and location
"ESO\nPortal" -> "ESO\nBackend": Initiate peering
"ESO\nBackend" -> "SIF\nController": Create connection to OSS
"SIF\nController" -> "ESO\nBackend": Connection created
"ESO\nBackend" -> "DO\nBackend": Authentication with credentials
"ESO\nBackend" -> "DO\nBackend": TMF service specifications import
"DO\nBackend" -> "ESO\nBackend": Imported services
"ESO\nBackend" -> -> "ESO\nPortal": Visualization of imported services
"ESO\nPortal" -> DomainOwner
DomainOwner -> "ESO\nPortal": Select desired services
"ESO\nPortal" -> "ESO\nBackend"
"ESO\nBackend" -> "DO\nBackend": Import filtering
"DO\nBackend" -> "ESO\nBackend": Filtered services inserted into service catalog
"DO\nBackend" -> "ESO\nBackend": Successful peering
"ESO\nBackend" -> -> "ESO\nPortal": Visualization of filtered services
"ESO\nPortal" -> DomainOwner: New DO appears on the map
"ESO\nPortal" -> DomainOwner: Service marketplace updated with imported services

note over "ESO\nPortal", "Cluster\nController": End of platform expansion view"

```
