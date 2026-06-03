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

User -> "BMP\nFrontend": Product Marketplace
"BMP\nFrontend" -> "BMP\nBackend": GET available Products
"BMP\nBackend" -> "BMP\nFrontend": Available Products

== Place Product Order ==

User -> "BMP\nFrontend": Place product order
"BMP\nFrontend" -> "BMP\nBackend":
"BMP\nBackend" -> "BMP\nBackend": Translate product to\nservice order
"BMP\nBackend" -> "ESO\nBackend": Place service order

loop#White #Gold Until order completed
    "BMP\nBackend" -> "ESO\nBackend": Poll Service order status
    "BMP\nBackend" -> "BMP\nBackend": Update Product order status
end

== View Product Order Status ==

User -> "BMP\nFrontend": View Product Order status
"BMP\nFrontend" -> "BMP\nBackend": GET Product Order status
"BMP\nBackend" -> "BMP\nFrontend": Product Order status
"BMP\nFrontend" -> User: Product Order status

```
