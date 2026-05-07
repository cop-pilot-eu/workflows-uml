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

' Target server to be connected to SIF
REMOTE_INFRA_BOX("Target VM")
    COP_PILOT_BOX_WITH_LOGO_INNER("Server")
        COP_PILOT_SERVICE("Target\nServer")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
END_REMOTE_INFRA_BOX()

' =====================================================
' CI/CD Pipeline: SIF (OpenZiti) VM Enrollment & Tunnel
' Connect a VM to the SIF zero-trust network
' =====================================================

== Trigger SIF Enrollment Pipeline ==

PltAdmin -> "Automation\nServer": Trigger pipeline manually\n(NODE_LABEL, JWT_FILENAME, TUNNEL_MODE)
"Automation\nServer" -> "Automation\nServer": Validate parameters\n(JWT file exists)

== Install Dependencies ==

"Automation\nServer" -> "Target\nServer": Install curl, jq, unzip, tar

== Install Ziti CLI ==

"Automation\nServer" -> "Target\nServer": Download & install latest\nziti CLI from GitHub

== Install ziti-edge-tunnel ==

"Automation\nServer" -> "Target\nServer": Download & install latest\nziti-edge-tunnel from GitHub

== Enroll Identity ==

"Target\nServer" -> "SIF\nController": ziti edge enroll\n(JWT → JSON identity)
"SIF\nController" -> "Target\nServer": Identity enrolled

== Start Ziti Tunnel ==

"Target\nServer" -> "SIF\nController": ziti-edge-tunnel run\n(systemd service or nohup)
"SIF\nController" -> "Target\nServer": Tunnel connected

== Verify Connectivity ==

"Automation\nServer" -> "Target\nServer": Check tunnel process running
"Target\nServer" -> "Automation\nServer": Tunnel active

note over "SIF\nController", "Target\nServer"
  Target Server identity now ONLINE
  (green) in CloudZiti Console.
  Server is connected to the SIF network.
end note

```
