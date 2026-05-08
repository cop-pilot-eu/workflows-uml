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

' Cluster X - Domain Y components (inlined)
REMOTE_INFRA_BOX("COP-PILOT Cluster X - Domain Y")
    COP_PILOT_BOX_WITH_LOGO_INNER("Domain\nOrchestrator (DO)")
        COP_PILOT_SERVICE("DO\nBackend")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
    CLUSTER_BOX_WITH_LOGO("Compute Infrastructure")
        CLUSTER_CTRL("Cluster\nController")
    END_CLUSTER_BOX_WITH_LOGO()
END_REMOTE_INFRA_BOX()

' =====================================================
' CI/CD Pipeline: OpenSlice (DO) Kubernetes Deployment
' via Jenkins Automation Server using Helm
' =====================================================

== Trigger OpenSlice K8s Deployment Pipeline ==

PltAdmin -> "Automation\nServer": Trigger pipeline manually\n(NODE_LABEL, GIT_BRANCH, ROOT_URL,\ncredentials, namespace)
"Automation\nServer" -> "Automation\nServer": Validate environment\n(kubectl, helm, git, cluster access)

== Clone OpenSlice Repository ==

"Automation\nServer" -> "Automation\nServer": git clone org.etsi.osl.main\n(selected branch)

== Configure Web UIs & Helm Values ==

"Automation\nServer" -> "Automation\nServer": Create config.js, config.prod.json,\ntheming.scss from defaults
"Automation\nServer" -> "Automation\nServer": Update values.yaml\n(rooturl, MySQL, Keycloak, CRIDGE)

== Ensure NGINX Ingress Controller ==

"Automation\nServer" -> "Cluster\nController": Install/upgrade NGINX Ingress\n(NodePort + Artemis TCP:61616)
"Cluster\nController" -> "Automation\nServer": Ingress Controller ready

== Deploy OpenSlice via Helm ==

"Automation\nServer" -> "Cluster\nController": helm upgrade --install openslice\n(namespace, chart, values)
"Cluster\nController" -> "Automation\nServer": Helm release deployed

== Wait for Pods ==

"Automation\nServer" -> "Cluster\nController": Poll pod status
"Cluster\nController" -> "Automation\nServer": All pods Running

== Disable Keycloak SSL (HTTP mode) ==

"Automation\nServer" -> "DO\nBackend": kubectl exec keycloak pod\n(kcadm.sh update sslRequired=NONE)
"DO\nBackend" -> "Automation\nServer": SSL disabled for\nopenslice & master realms

== Smoke Test ==

"Automation\nServer" -> "DO\nBackend": GET /tmf-api/serviceCatalogManagement\n/v4/serviceCatalog
"DO\nBackend" -> "Automation\nServer": TMF API responding (HTTP 200)

```
