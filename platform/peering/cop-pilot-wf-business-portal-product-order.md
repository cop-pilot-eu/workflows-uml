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

' Business Portal components used in this workflow
COP_PILOT_BOX_WITH_LOGO_INNER("Business\nPortal")
    COP_PILOT_SERVICE("Business\nPortal UI")
    COP_PILOT_SERVICE("Business\nPortal API")
    COP_PILOT_SERVICE("Business\nPortal New\nComponent")
END_COP_PILOT_BOX_WITH_LOGO_INNER()

' External authentication component
participant "Authentication\nEntity" as AuthenticationEntity #GREY_3

' =====================
' Business Portal Product
' Ordering Flow
' =====================

== Authenticate user ==

User -> AuthenticationEntity: Authentication request
AuthenticationEntity -> User: Authentication successful

== Browse Product Marketplace ==

User -> "Business\nPortal UI": Product Marketplace
"Business\nPortal UI" -> "Business\nPortal API": GET available Products
"Business\nPortal API" -> "Business\nPortal UI": Available Products

== Place Product Order ==

User -> "Business\nPortal UI": Place Product Order
"Business\nPortal UI" -> "Business\nPortal API": POST Product Order
"Business\nPortal API" -> "Business\nPortal New\nComponent": Translate Product to\nService order
"Business\nPortal New\nComponent" -> "ESO\nBackend": Place Service Order

loop Until order completed
    "Business\nPortal New\nComponent" -> "ESO\nBackend": Poll Service order status
    "Business\nPortal New\nComponent" -> "Business\nPortal API": Update Product order status
end

== View Product Order Status ==

User -> "Business\nPortal UI": View Product Order status
"Business\nPortal UI" -> "Business\nPortal API": GET Product Order status
"Business\nPortal API" -> "Business\nPortal UI": Product Order status
"Business\nPortal UI" -> User: Product Order status

```
