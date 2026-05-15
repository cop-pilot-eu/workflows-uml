```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Domain\nOwner" as DomainOwner #000000
actor "Platform\nAdmin" as PltAdmin #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' General template providing COP-PILOT Cluster components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-cluster-generic.puml

' =====================
' Platform expansion to new private domain
' Step 1
' =====================

note over "ESO\nPortal", "ESO\nBackend": This flow initiates platform expansion to a new private domain.\nIt assumes the ESO has already peered with the SIF platform.

== Step 0: Registration of the domain owner ==

DomainOwner -> PltAdmin: Request a COP-PILOT ESO account
PltAdmin -> DomainOwner: Ask for relevant user information
DomainOwner -> PltAdmin: Provide the requested info
PltAdmin -> "ESO\nPortal": Create user and assign user to group
"ESO\nPortal" -> "ESO\nBackend"
"ESO\nPortal" -> "ESO\nBackend": Create a TMF organization party\n(if user belongs to an organization)
"ESO\nBackend" -> "ESO\nPortal": New user (with potentially associated TMF party) is created
"ESO\nPortal" -> PltAdmin
PltAdmin -> DomainOwner: Share credentials with new domain owner

note over "ESO\nPortal", "ESO\nBackend": Go to step 1 "Platform expansion request"

```
