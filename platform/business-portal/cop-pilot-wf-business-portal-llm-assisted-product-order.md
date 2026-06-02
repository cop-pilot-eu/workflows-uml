```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "User" as User #000000

' Business Portal components used in this workflow
COP_PILOT_BOX_WITH_LOGO_INNER("Business\nPortal")
    COP_PILOT_SERVICE("Business\nPortal UI")
    COP_PILOT_SERVICE("LLM-assisted\nOrdering Assistant")
    COP_PILOT_SERVICE("Business\nPortal API")
    COP_PILOT_SERVICE("Business\nPortal\nBack-End")
END_COP_PILOT_BOX_WITH_LOGO_INNER()

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' Keep loop / group fragments readable with the shared dark theme
skinparam SequenceGroupBackgroundColor #F0F0F0
skinparam SequenceGroupBodyBackgroundColor #FFFFFF
skinparam SequenceGroupBorderColor #000000
skinparam SequenceGroupFontColor #000000
skinparam SequenceGroupHeaderFontColor #000000
skinparam sequence {
    GroupBackgroundColor #F0F0F0
    GroupBodyBackgroundColor #FFFFFF
    GroupBorderColor #000000
    GroupFontColor #000000
    GroupHeaderFontColor #000000
}

' External authentication component
participant "Authentication\nEntity" as AuthenticationEntity #GREY_3

' =====================
' LLM-assisted Product
' Ordering Flow
' =====================

== Authenticate user ==

User -> AuthenticationEntity: Authentication request
AuthenticationEntity -> User: Authentication successful

== Start assisted Product ordering ==

User -> "Business\nPortal UI": Describe Product need\nin natural language
"Business\nPortal UI" -> "LLM-assisted\nOrdering Assistant": Start guided Product ordering
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal API": GET available Products
"Business\nPortal API" -> "LLM-assisted\nOrdering Assistant": Available Products
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal UI": Recommend matching Products\nand required parameters
"Business\nPortal UI" -> User: Guided Product selection

== Refine and validate Product order ==

User -> "Business\nPortal UI": Select Product and provide intent
"Business\nPortal UI" -> "LLM-assisted\nOrdering Assistant": Refine Product order request
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal API": Validate Product order\n(options, constraints, eligibility)
"Business\nPortal API" -> "LLM-assisted\nOrdering Assistant": Validation result
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal UI": Product order summary\nand confirmation request
"Business\nPortal UI" -> User: Confirm Product order

== Place Product order ==

User -> "Business\nPortal UI": Confirm order
"Business\nPortal UI" -> "LLM-assisted\nOrdering Assistant": Submit confirmed order
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal API": POST Product Order
"Business\nPortal API" -> "Business\nPortal\nBack-End": Translate Product to\nService order
"Business\nPortal\nBack-End" -> "ESO\nBackend": Place Service Order
"ESO\nBackend" -> "Business\nPortal\nBack-End": Service Order accepted
"Business\nPortal\nBack-End" -> "Business\nPortal API": Product Order created
"Business\nPortal API" -> "LLM-assisted\nOrdering Assistant": Product Order reference
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal UI": Product Order submitted
"Business\nPortal UI" -> User: Order confirmation

== Track Product order ==

loop Until order completed
    "Business\nPortal\nBack-End" -> "ESO\nBackend": Poll Service order status
    "ESO\nBackend" -> "Business\nPortal\nBack-End": Service order status
    "Business\nPortal\nBack-End" -> "Business\nPortal API": Update Product order status
end

User -> "Business\nPortal UI": Ask assistant for order status
"Business\nPortal UI" -> "LLM-assisted\nOrdering Assistant": Retrieve Product order status
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal API": GET Product Order status
"Business\nPortal API" -> "LLM-assisted\nOrdering Assistant": Product Order status
"LLM-assisted\nOrdering Assistant" -> "Business\nPortal UI": Explain status and next steps
"Business\nPortal UI" -> User: Product Order status

```
