```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Domain Owner" as User #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' General template providing COP-PILOT Cluster components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-cluster-generic.puml

' =====================
' Service update flow
' for single-domain services
' =====================

note over "DO\nPortal", "DO\nBackend": This flow assumes that domain infrastructure controllers are registered as TMF Resource Specifications 

== Single-domain service design flow ==

User -> "DO\nPortal": Browse available resource specifications
User -> "DO\nPortal": Design resource facing service specification(s)\n (relationship with resource specifications)
User -> "DO\nPortal": Design customer facing service specification(s)\n (relationship with designed resource facing service specifications)
User -> "DO\nPortal": Define customer facing characteristics
User -> "DO\nPortal": Define lifecycle rules for the designed service specification(s)
User -> "DO\nPortal": Finalise the service specification(s) design
"DO\nPortal" -> "DO\nBackend": Save the specification(s)
User -> "DO\nPortal": Organise specifications in service catalogs/categories
"DO\nPortal" -> "DO\nBackend": Save changes / update service Μarketplace

```
