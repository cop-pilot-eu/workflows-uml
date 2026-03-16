# COP-PILOT's common workflow UML library

This repository creates a UML library dedicated to [COP-PILOT](https://cop-pilot.eu/) and its main platform components.
The purpose of this library is to be used by all COP-PILOT stakeholders to design sequence diagrams that concern:
- Internal interactions among platform components
- Interactions between COP-PILOT stakeholders and the platform
- Interactions of the COP-PILOT platform and the various Cluster infrastructures.

To exploit this common UML librady and design your desired workflow, please follow the steps below.

## Install VSCode with extensions
- Install [VSCode](https://code.visualstudio.com/)
- Install the following VSCode Extensions (Shift+Ctrl+X):
  - Markdown All in One (`yzhang.markdown-all-in-one`)
  - PlantUML (`jebbs.plantuml`)

## Configure VSCode
Go to File > Preferences > Settings:
- Select "User" tab to apply the settings to user-wide VSCode or "Workspace" tab to apply them to this workspace only.
  - Both options are valid; it depends if you want to inherit these settings in other workspaces.
- Find the following settings using the search box and apply the values indicated:
  - `plantuml.server` : `https://www.plantuml.com/plantuml`
  - `plantuml.render` : `PlantUMLServer`

## Preview a Markdown file in VSCode

Open the example workflow file `cop-pilot-workflow-example.md`

Right click on it and click `Open Preview` or press `Shift+Ctrl+V`.

Shortly you will get the rendered Markdown page with the rendered diagram figure.
You can extend this example or generate a similar file with your preferred workflow.
