```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "User" as User #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' ====================================
' LLM-assisted Product Ordering Flow
' ====================================

== Authenticate User ==

User -> "BMP\nFrontend": Login
"BMP\nFrontend" -> User: Successful login

== Start Assisted Product Ordering ==

User -> "BMP\nFrontend": Describe product need\nin natural language
"BMP\nFrontend" -> "BMP\nBackend": Start guided product ordering
"BMP\nBackend" -> "BMP\nBackend": Get available products
"BMP\nBackend" -> "BMP\nFrontend": Recommend matching products\nand required parameters
"BMP\nFrontend" -> User: Guided product selection

== Refine and Validate Product Order ==

User -> "BMP\nFrontend": Select product and provide intent
"BMP\nFrontend" -> "BMP\nBackend": Refine product order request
"BMP\nBackend" -> "BMP\nBackend": Validate product order\n(options, constraints, eligibility)
"BMP\nBackend" -> "BMP\nFrontend": Product order summary\nand confirmation request
"BMP\nFrontend" -> User: Confirm product order

== Place Product Order ==

User -> "BMP\nFrontend": Confirm product order
"BMP\nFrontend" -> "BMP\nBackend": Submit confirmed product order
"BMP\nBackend" -> "BMP\nBackend": Create product order
"BMP\nBackend" -> "BMP\nBackend": Translate product to\nservice order
"BMP\nBackend" -> "ESO\nBackend": Place service order
"ESO\nBackend" -> "BMP\nBackend": Service order accepted
"BMP\nBackend" -> "BMP\nBackend": Product order created
"BMP\nBackend" -> "BMP\nFrontend": Product order submitted
"BMP\nFrontend" -> User: Product order confirmation

== Track Product Order ==

loop#White #Gold Until order completed
    "BMP\nBackend" -> "ESO\nBackend": Poll service order status
    "ESO\nBackend" -> "BMP\nBackend": Service order status
    "BMP\nBackend" -> "BMP\nBackend": Update product order status
end

User -> "BMP\nFrontend": Ask assistant for order status
"BMP\nFrontend" -> "BMP\nBackend": Retrieve product order status
"BMP\nBackend" -> "BMP\nBackend": Get product order status
"BMP\nBackend" -> "BMP\nBackend": Product order status
"BMP\nBackend" -> "BMP\nFrontend": Explain status and next steps
"BMP\nFrontend" -> User: Product order status

```
