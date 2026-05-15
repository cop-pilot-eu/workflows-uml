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

' General template providing COP-PILOT Cluster components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-cluster-generic.puml

' =====================
' Federation of distributed
' domain service marketplaces
' =====================

== Federation of Distributed Domain Service Marketplaces ==

PltAdmin -> "ESO\nPortal": Browse on Platform OSS view
PltAdmin -> "ESO\nPortal": Add new OSS
PltAdmin -> "ESO\nPortal": Configure OSS endpoint, credentials, and location
"ESO\nPortal" -> "ESO\nBackend": Create a TMF organization party for remote DO
"ESO\nBackend" -> "ESO\nPortal": TMF organization party created
"ESO\nPortal" -> "ESO\nBackend": Initiate peering
"ESO\nBackend" -> "DO\nBackend": Authenticate with credentials
"ESO\nBackend" -> "DO\nBackend": Retrieve service specifications
"DO\nBackend" -> "ESO\nBackend": Return service specifications
"ESO\nBackend" -> "ESO\nPortal": Visualize retrieved service specifications
"ESO\nPortal" -> PltAdmin
PltAdmin -> "ESO\nPortal": Select desired services to import
"ESO\nPortal" -> "ESO\nBackend"
"ESO\nBackend" -> "DO\nBackend": Request service specification details
"DO\nBackend" -> "ESO\nBackend": Service specification details retrieved
"ESO\nBackend" -> "ESO\nPortal": Successful peering
"ESO\nPortal" -> PltAdmin: Service marketplace updated with imported services
"ESO\nPortal" -> PltAdmin: New DO appears on the map

```
