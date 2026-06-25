```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Service\nProvider" as SP #000000

' CI/CD Platform box: source control, registry and the CD plugin
COP_PILOT_BOX_OUTER("CI/CD Platform")
    COP_PILOT_BOX_WITH_LOGO_INNER("CI/CD")
        COP_PILOT_SERVICE("GitHub")
        COP_PILOT_SERVICE("Artefact\nRegistry")
        COP_PILOT_SERVICE("CD\nPlugin")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
END_COP_PILOT_BOX_OUTER()

' Test environment (validation) and production environment (DO/ESO)
REMOTE_INFRA_BOX("COP-PILOT Test Environment")
    COP_PILOT_BOX_WITH_LOGO_INNER("Orchestrator (DO/ESO)")
        COP_PILOT_SERVICE("Test\nOrchestrator")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
END_REMOTE_INFRA_BOX()

REMOTE_INFRA_BOX("COP-PILOT Production Cluster")
    COP_PILOT_BOX_WITH_LOGO_INNER("Orchestrator (DO/ESO)")
        COP_PILOT_SERVICE("Prod\nOrchestrator")
    END_COP_PILOT_BOX_WITH_LOGO_INNER()
END_REMOTE_INFRA_BOX()

' =====================================================
' Automated deployment of Cluster vertical applications
' upon every update:
'   Stage 1 - build and validation
'   Stage 2 - release management
'   Stage 3 - deployment validation through orchestrators
' =====================================================

== Stage 1: Build and validation ==

SP -> "GitHub": Push commit\n(via webhook or manual trigger)
"GitHub" -> "GitHub": Clone, lint, compile,\nunit test
"GitHub" -> SP: Report build/validation status

== Stage 2: Release management ==

SP -> "GitHub": Tag commit\n(via webhook or manual trigger)
"GitHub" -> "GitHub": Build image,\npackage Helm chart
"GitHub" -> "Artefact\nRegistry": Push image and chart\n(versioned artefacts)
"Artefact\nRegistry" -> SP: Report release outcome

== Stage 3: Deployment validation through the orchestrators ==

"Artefact\nRegistry" -> "CD\nPlugin": Webhook (artefact published)
"CD\nPlugin" -> "Artefact\nRegistry": Fetch published artefacts

== Preflight checks ==

"CD\nPlugin" -> "CD\nPlugin": Lint manifests, validate OCI\nreferences & deployment descriptors
"CD\nPlugin" -> SP: Report preflight check result

== Collect deployment parameters ==

"CD\nPlugin" -> "Test\nOrchestrator": Check for running instance\n(TMF638 Service Inventory)

alt Test instance is running
    "Test\nOrchestrator" -> "CD\nPlugin": Service inventory record\n(configuration for redeploy)
else No test instance running
    "CD\nPlugin" -> "Prod\nOrchestrator": Get service specification\n(TMF633)
    "Prod\nOrchestrator" -> "CD\nPlugin": Service specification settings
end

== Validate in test environment ==

"CD\nPlugin" -> "Test\nOrchestrator": Submit Service Test\n(TMF653) for new release
"Test\nOrchestrator" -> "Artefact\nRegistry": Fetch image
"Artefact\nRegistry" -> "Test\nOrchestrator": Image
"Test\nOrchestrator" -> "CD\nPlugin": Smoke test result\n(startup, connectivity, interfaces)
"CD\nPlugin" -> SP: Report smoke test result

== Promote to production ==

"CD\nPlugin" -> "Prod\nOrchestrator": Submit Service Order\n(TMF641) for updated release

"Prod\nOrchestrator" -> "Artefact\nRegistry": Fetch image
"Artefact\nRegistry" -> "Prod\nOrchestrator": Image
"Prod\nOrchestrator" -> "Prod\nOrchestrator": Provision resources\n& deploy service
"Prod\nOrchestrator" -> "CD\nPlugin": Order status\n(tracked to terminal state)
"CD\nPlugin" -> SP: Notify result\n(success or classified failure)

```
