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

' ==========================
' BMP Product Ordering Flow
' ==========================

== Authenticate User ==

User -> "BMP\nFrontend": Login
"BMP\nFrontend" -> User: Successful login

== Browse Product Marketplace ==

User -> "BMP\nFrontend": Browse product marketplace
"BMP\nFrontend" -> "BMP\nBackend": Get available products
"BMP\nBackend" -> "BMP\nFrontend": Available products

== Place Product Order ==

User -> "BMP\nFrontend": Place product order
"BMP\nFrontend" -> "BMP\nBackend":
"BMP\nBackend" -> "BMP\nBackend": Validate product order
"BMP\nBackend" -> "BMP\nBackend": Translate product to\nservice order
"BMP\nBackend" -> "ESO\nBackend": Place service order

loop#White #Gold Until order completed
    "BMP\nBackend" -> "ESO\nBackend": Poll service order status
    "BMP\nBackend" -> "BMP\nBackend": Update product order status
end

== View Product Order Status ==

User -> "BMP\nFrontend": View product order status
"BMP\nFrontend" -> "BMP\nBackend": Get product order status
"BMP\nBackend" -> "BMP\nFrontend": Product order status
"BMP\nFrontend" -> User:

```
