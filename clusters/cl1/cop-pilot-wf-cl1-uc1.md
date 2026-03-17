```plantuml

' Uncomment this at the end of your design to see only the linked components
' hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Service\nProvider" as SvcPrv #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' Cluster and use-case specific templates designed particularly for a given scenario
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/clusters/cl1/components-cl1-uc1.puml

' =====================
' CL1 - UC1 flow
' =====================
SvcPrv -> "LLM-enhanced\nPortal": Ask for available domain\nservices for Cluster 1

```
