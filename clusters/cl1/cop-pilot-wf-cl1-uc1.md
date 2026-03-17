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

== Chat with the COP-PILOT LLM to identify desired services ==

SvcPrv -> "LLM-enhanced\nPortal": Ask for available domain\nservices for Cluster 1
"LLM-enhanced\nPortal" -> "ESO\nBackend": Get Cluster 1 services
"ESO\nBackend" -> "LLM-enhanced\nPortal": 
"LLM-enhanced\nPortal" -> SvcPrv: 

== Order a specific service via the respective orchestrator ==

SvcPrv -> "LLM-enhanced\nPortal": Order Cluster 1 reconciliation service
"LLM-enhanced\nPortal" -> "DO\nBackend": Reconciliation service order
"DO\nBackend" -> "Cluster\nController": Deploy reconciliation service
"Cluster\nController" -> "DO\nBackend": Deployed
"DO\nBackend" -> "LLM-enhanced\nPortal": Service order completed
"LLM-enhanced\nPortal" -> SvcPrv: Service order completed

```
