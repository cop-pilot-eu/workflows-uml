```plantuml

' Define the stakeholders of this workflow
actor "Vertical\nEnd User" as VertEndUser #000000
actor "Infra.\nProvider" as InfPrv #000000
actor "Service\nProvider" as SvcPrv #000000
actor "Platform\nAdmin" as PltAdmin #000000

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' Cluster and use-case specific templates designed particularly for a given scenario
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/clusters/cl1/components-cl1-uc1.puml

' =====================
' Example Flow
' =====================
SvcPrv -> LLM: Ask for available domain\nservices for Cluster 1

```
