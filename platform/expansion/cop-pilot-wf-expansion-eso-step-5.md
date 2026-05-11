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
' Step 5
' =====================

note over "ESO\nPortal", "Cluster\nController": This flow follows up platform expansion step 4 "Order a new COP-PILOT DO for this domain"

== Step 5: COP-PILOT DO resource discovery ==

note over "DO\nBackend", "5G\nController": Resource discovery and exposure

note over "ESO\nPortal", "Cluster\nController": Go to step 6 "COP-PILOT ESO-DO peering"

```
