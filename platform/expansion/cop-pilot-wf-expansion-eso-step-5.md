```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' General template providing COP-PILOT Cluster components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-cluster-generic.puml

' =====================
' Platform expansion to new private domain
' Step 5
' =====================

note over "DO\nPortal", "5G\nController": This flow follows up platform expansion step 4 "Order a new COP-PILOT DO for this domain"

== Step 5: COP-PILOT DO resource discovery, service design, and exposure ==


"Cluster\nController" -> "DO\nBackend": Compute Infrastructure Controller discovered\n(details provided at the new COP-PILOT DO request)

"5G\nController" -> "DO\nBackend": Network Infrastructure Controller discovered\n(details provided at the new COP-PILOT DO request)

"DO\nBackend" -> "DO\nPortal": Infrastructure Controllers\n presented as TMF Resource Specifications

```
