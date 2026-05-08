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
END_REMOTE_INFRA_BOX()

' =====================================================
' CI/CD Pipeline: Expose OpenSlice (DO) via OpenZiti (SIF)
' Creates Ziti service, configs & policies
' =====================================================

== Trigger OpenZiti Service Creation Pipeline ==

PltAdmin -> "Automation\nServer": Trigger pipeline manually\n(BASE_NAME, SERVICE_IP, SERVICE_PORT,\nHOST_IDENTITY_NAME, PROTOCOLS)
"Automation\nServer" -> "Automation\nServer": Pipeline started\n(validate parameters)

== Ziti Login ==

"Automation\nServer" -> "SIF\nController": ziti edge login
"SIF\nController" -> "Automation\nServer": Authenticated

== Create Intercept Config ==

"Automation\nServer" -> "SIF\nController": Create intercept.v1 config\n(address: SERVICE_IP, port: SERVICE_PORT)
"SIF\nController" -> "Automation\nServer": Intercept config created

== Create Host Config ==

"Automation\nServer" -> "SIF\nController": Create host.v1 config\n(forward to SERVICE_IP:SERVICE_PORT)
"SIF\nController" -> "Automation\nServer": Host config created

== Create Ziti Service ==

"Automation\nServer" -> "SIF\nController": Create service\n(attach intercept + host configs)
"SIF\nController" -> "Automation\nServer": Service created

== Create Service Policies ==

"Automation\nServer" -> "SIF\nController": Create Dial policy\n(fixed authorized identities)
"SIF\nController" -> "Automation\nServer": Dial policy created
"Automation\nServer" -> "SIF\nController": Create Bind policy\n(HOST_IDENTITY_NAME)
"SIF\nController" -> "Automation\nServer": Bind policy created

== OpenSlice (DO) exposed securely via SIF ==

note over "SIF\nController", "DO\nBackend"
  OpenSlice endpoint now accessible
  only to authorized Ziti identities
  via SERVICE_IP:SERVICE_PORT
end note

```
