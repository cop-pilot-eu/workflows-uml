```plantuml

' Uncomment this at the end of your design to see only the linked components
hide unlinked

' General templates useful for every workflow
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/palette.puml
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/theme.puml

' Define the stakeholders of this workflow
actor "Business\nPortal Admin" as BMPAdmin #000000

' General template providing the central COP-PILOT components
!includeurl https://raw.githubusercontent.com/cop-pilot-eu/workflows-uml/refs/heads/main/templates/components-central.puml

' ==================================
' BMP Product Creation and Exposure
' ==================================

== Authenticate administrator ==

BMPAdmin -> "BMP\nFrontend": Login
"BMP\nFrontend" -> BMPAdmin: Successful login

== Create new Product specification and offering ==

BMPAdmin -> "BMP\nFrontend": Create new Product\n(specification and offering)
"BMP\nFrontend" -> "ESO\nBackend": Retrieve available\nservice specifications
"ESO\nBackend" -> "BMP\nFrontend": Available service specifications

== Save new Product ==

BMPAdmin -> "BMP\nFrontend": Save new product
"BMP\nFrontend" -> "BMP\nBackend": Create product\n(reference selected ESO service spec.)
"BMP\nBackend" -> "BMP\nFrontend": Product created
"BMP\nFrontend" -> BMPAdmin: Product visualized

== Expose new Product to Catalog ==

BMPAdmin -> "BMP\nFrontend": Expose new product to product catalog
"BMP\nFrontend" -> "BMP\nBackend":
"BMP\nBackend" -> "BMP\nFrontend": Product exposed to product catalog
"BMP\nFrontend" -> BMPAdmin:

```
