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

' =====================
' Peering Flow between
' ESO and SIF
' =====================

== Create ESO account on SIF ==

PltAdmin -> "SIF\nController": Issue new account for ESO
"SIF\nController" -> PltAdmin: ESO account created

== Authenticated peering between ESO and SIF ==

PltAdmin -> "ESO\nPortal": Add SIF network
"ESO\nPortal" -> "ESO\nBackend": Create network
"ESO\nBackend" -> "SIF\nController": Authenticate with ESO credentials
"SIF\nController" -> "ESO\nBackend": Successful authentication
"ESO\nBackend" -> "ESO\nPortal": Network created
"ESO\nPortal" -> PltAdmin: SIF network added

== Test ESO and SIF interaction ==

PltAdmin -> "ESO\nPortal": Create 'test' SIF identity
"ESO\nPortal" -> "ESO\nBackend": Create identity
"ESO\nBackend" -> "SIF\nController": Create identity
"SIF\nController" -> "ESO\nBackend": Identity created
"ESO\nBackend" -> "ESO\nPortal": Identity created
"ESO\nPortal" -> PltAdmin: 'test' SIF identity created

```
