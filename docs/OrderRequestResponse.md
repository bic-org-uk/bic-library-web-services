![BIC LOGO](assets/bic-logo.png)

### *Book Industry Communication*

### BIC Library Web Services API Standards

# Order Request and Order Response

**Version 0.9, 28 September 2018**

**The published version of this document:** <http://www.bic.org.uk/files/pdfs/BICLWSOrderRequestResponse-V0.9.pdf>  
**XML schema:** <http://www.bic.org.uk/files/xml/BICLWSOrderRequestResponse-V0.9.xsd>  
**WSDL file:**  <http://www.bic.org.uk/files/xml/BICLWSOrderRequestResponseSOAP-V0.9.wsdl>  
**XML namespace:** http://www.bic.org.uk/librarywebservices/Order  
**Next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Order API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

An Order Request may be implemented using either SOAP or the basic HTTPS
protocol\[1\](#Notes1) and POST method. The payload of an Order Request may be
formatted either as an XML document or as an equivalent JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTPS and SOAP exchanges.

The complete specification of the Order Request/Response web service
includes two machine-readable resources that are to be used by
implementers in conjunction with this document:

  - a WSDL Definition for the SOAP protocol version of the web service

  - an XML schema for Requests and Response payloads in XML format.

A Request or Response payload expressed in JSON must be entirely
translatable into an equivalent XML representation that conforms to the
XML schema.

It is strongly recommended that SOAP client implementations of this web
service be constructed using the BIC WSDL Definitions as a starting
point, as this will promote interoperability between SOAP client and
server implementations. In some development environments it may be
easier to implement a SOAP server without using the BIC WSDL
Definitions, but in this case care must be taken to ensure that the WSDL
Definitions that describe the actual implementation is functionally
equivalent to the BIC WSDL Definitions.

These formats are closely based upon the BIC Realtime Order
Request/Response API and the EDItX Library Order and draft EDItX Order
Response formats developed by EDItEUR.

### Business requirements

This service will enable the use of web services for the placing of
orders by libraries (or possibly, in appropriate cases, their agents)
with wholesalers or distributors, or possibly the placing of orders by
wholesalers with distributors, where this complements existing
paper-based and electronic ordering methods. Using this service, the
buyer can receive an immediate response to the order and not simply a
technical acknowledgement of receipt.

This service does not currently support requests for new standing
orders.

An ordering service typically needs to communicate with other existing
systems to determine the correct response to an order request. The
EDItEUR order line status code list (see Table 1 on page 22) has been
extended to enable a wider range of responses, as are provided by
systems that implement existing EDI standards.

### REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**XML document encoding begins:** `<OrderRequest version="0.9">...`

**JSON document encoding begins:** `{ "OrderRequest": { "version": "0.9"...`

<table>
<tbody>
<tr valign="top">
  <th></th>
  <th>Description</th>
  <th><a href="#Notes2">[2]</a></th>
  <th>XML tag / JSON field name</th>
  <th><a href="#Notes3">[3]</a></th>
</tr>
<tr valign="top">
<td colspan="5"><h4>Header</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Request header</strong></p></td>
<td><p><strong>M</strong></p></td>
<td><p><strong>Header</strong></p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>A unique identifier for the sender of the request. An alphanumeric string not containing spaces or punctuation.</p></td>
<td><p>D</p></td>
<td><pre>ClientID</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>A password to further authenticate the sender of the request, if required in the payload<a href="#Notes4">[4]</a>.</p></td>
<td><p>D</p></td>
<td><pre>ClientPassword</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Account identifier for this request</p></td>
<td><p>D</p></td>
<td><pre>AccountIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier. Mandatory if including an account identifier. Permitted values are:</p>
<ul><li><em>01</em> Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li>
<li><em>11</em>&nbsp;&nbsp;PubEasy PIN</li></ul></td>
<td><p>M</p></td>
<td><pre>  AccountIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Request number</p></td>
<td><p>D</p></td>
<td><pre>RequestNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Purchase order number for this order request. Mandatory in all order requests.</p></td>
<td><p>M</p></td>
<td><pre>OrderNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Document date/time: the date/time when the request was generated. Permitted formats are:</p>
<ul><li>YYYYMMDD</li>
<li>YYYYMMDDTHHMM</li>
<li>YYYYMMDDTHHMMZ (universal time)</li>
<li>YYYYMMDDTHHMM±HHMM (time zone)</li></ul>
<p>where "T" represents itself, ie letter T</p></td>
<td><p>D</p></td>
<td><pre>IssueDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Order references. If included, must contain a reference number or a reference date or both.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>17</em>&nbsp;&nbsp;Promotion or deal reference</li>
<li><em>24</em>&nbsp;&nbsp;Order source location reference</li>
<li><em>29</em>&nbsp;&nbsp;Supplier quotation reference</li>
<li><em>32</em>&nbsp;&nbsp;Authorisation for expense reference</li>
<li><em>35</em>&nbsp;&nbsp;Library’s supplier reference</li>
<li><em>36</em>&nbsp;&nbsp;Supplier’s library customer reference</li>
<li><em>37</em>&nbsp;&nbsp;Library’s additional order reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date-time (for format options see line 6)</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Order type code</p>
<ul><li><em>01</em>&nbsp;&nbsp;New order (default)</li>
<li><em>02</em>&nbsp;&nbsp;Order for approval or inspection copies</li>
<li><em>03</em>&nbsp;&nbsp;Confirmation order (confirming an order placed on a supplier’s website)</li></ul></td>
<td><p>D</p></td>
<td><pre>OrderTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Currency in which the requester would prefer prices to be quoted</p></td>
<td><p>D</p></td>
<td><pre>CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Order qualifying dates</p></td>
<td><p>D</p></td>
<td><pre>DateCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Date YYYYMMDD</p></td>
<td><p>M</p></td>
<td><pre>  Date</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Date qualifier</p>
<ul><li><em>01</em>&nbsp;&nbsp;Cancel if not shipped by date</li>
<li><em>02</em>&nbsp;&nbsp;Cancel if not shipped by date, unless not yet published</li>
<li><em>03</em>&nbsp;&nbsp;Fill all available by date</li>
<li><em>04</em>&nbsp;&nbsp;Do not ship before date</li></ul></td>
<td><p>M</p></td>
<td><pre>  DateQualifierCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>12</td>
<td><p>Order fill terms</p>
<ul><li><em>01</em>&nbsp;&nbsp;Fill all or kill all</li>
<li><em>02</em>&nbsp;&nbsp;Fill all or backorder all</li>
<li><em>03</em>&nbsp;&nbsp;Fill available, kill remainder</li>
<li><em>04</em>&nbsp;&nbsp;Fill available, kill remainder unless not yet published</li>
<li><em>05</em>&nbsp;&nbsp;Fill available, backorder remainder and ship when complete</li>
<li><em>06</em>&nbsp;&nbsp;Fill available, backorder remainder and ship as available</li></ul></td>
<td><p>D</p></td>
<td><pre>FillTermsCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Supplier to whom this order request should be forwarded, if it is not addressed to the web service host (use only for requests sent to aggregators).</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier ID type - see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>  SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Deliver to party (if not requester) – must include an identifier, a name or both.</p></td>
<td><p>D</p></td>
<td><pre>ShipToParty</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Party identifier</p></td>
<td><p>D</p></td>
<td><pre>  PartyIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Party ID type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li></ul></td>
<td><p>M</p></td>
<td><pre>    PartyIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier string</p></td>
<td><p>M</p></td>
<td><pre>    IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Party name</p></td>
<td><p>D</p></td>
<td><pre>  PartyName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Postal address</p></td>
<td><p>D</p></td>
<td><pre>  PostalAddress</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Address line</p></td>
<td><p>M</p></td>
<td><pre>    AddressLine</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Communication details</p></td>
<td><p>D</p></td>
<td><pre>  CommunicationDetails</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Communication type</p>
<ul><li><em>01</em>&nbsp;&nbsp;‘Landline’ phone</li>
<li><em>02</em>&nbsp;&nbsp;Mobile phone</li>
<li><em>03</em>&nbsp;&nbsp;Fax</li>
<li><em>04</em>&nbsp;&nbsp;Email</li>
<li><em>05</em>&nbsp;&nbsp;Web</li></ul></td>
<td><p>M</p></td>
<td><pre>    CommunicationTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Communication locator string</p></td>
<td><p>M</p></td>
<td><pre>    CommunicationLocator</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Contact details</p></td>
<td><p>D</p></td>
<td><pre>  ContactPerson</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Contact person’s name</p></td>
<td><p>M</p></td>
<td><pre>    PersonName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>15</p></td>
<td><p>Invoice to party (if not requester) – same structure as deliver to party.</p></td>
<td><p>D</p></td>
<td><pre>BillToParty</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>16</p></td>
<td><p>Time and means of delivery</p></td>
<td><p>D</p></td>
<td><pre>Delivery</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Requested delivery time</p>
<ul><li><em>01</em> Next day</li></ul>
<p>(Other codes to be added.)</p></td>
<td><p>D</p></td>
<td><pre>  DeliveryTimeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Use supplier’s delivery service – code or name assigned by supplier</p></td>
<td><p>D</p></td>
<td><pre>  VendorDeliveryService</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Use specified carrier</p></td>
<td><p>D</p></td>
<td><pre>  Carrier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Coded carrier name</p></td>
<td><p>D</p></td>
<td><pre>    CarrierNameCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Coding scheme type:</p>
<ul><li><em>01</em>&nbsp;&nbsp;BIC scheme</li>
<li><em>02</em>&nbsp;&nbsp;Supplier’s scheme</li>
<li><em>03</em>&nbsp;&nbsp;Buyer’s scheme</li></ul></td>
<td><p>M</p></td>
<td><pre>      CarrierNameCodeType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Carrier name code</p></td>
<td><p>M</p></td>
<td><pre>      CarrierNameCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Name of carrier</p></td>
<td><p>D</p></td>
<td><pre>    CarrierName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Carrier’s delivery service code / name</p></td>
<td><p>D</p></td>
<td><pre>    CarrierService</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Free text delivery instruction</p></td>
<td><p>D</p></td>
<td><pre>  DeliveryNotes</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>17</td>
<td><p>Shipping instructions.</p>
<ul><li><em>00</em>&nbsp;&nbsp;Follow standing instructions (default)</li>
<li><em>01</em>&nbsp;&nbsp;Ship this order separately without delay</li>
<li><em>02</em>&nbsp;&nbsp;Allocate stock now, pending shipping request or standing instruction</li>
<li><em>03</em>&nbsp;&nbsp;Ship this order without delay, combined with any backorder items awaiting shipping.</li></ul>
<p>NOTE – Shipping may be triggered by an order, by a shipping request or by a standing instruction to ship when agreed conditions are met (e.g. the value or weight of items awaiting shipping exceeds an agreed amount). The default is to follow standing instructions.</p></td>
<td><p>D</p></td>
<td><pre>ShippingInstructionsCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Cataloguing instructions</p></td>
<td><p>D</p></td>
<td><pre>CatalogingInstructions</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Catalogue format</p>
<ul><li><em>01</em>&nbsp;&nbsp;MARC21 ISO 2709</li>
<li><em>02</em>&nbsp;&nbsp;MARC21 XML</li>
<li><em>03</em>&nbsp;&nbsp;MODS</li>
<li><em>04</em>&nbsp;&nbsp;Dublin Core (version as per trading partner agreement)</li></ul></td>
<td><p>D</p></td>
<td><pre>  CatalogingFormatCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Cataloguing supply instruction</p>
<ul><li><em>02</em>&nbsp;&nbsp;Send with ship notice</li>
<li><em>03</em>&nbsp;&nbsp;Send with invoice</li></ul></td>
<td><p>D</p></td>
<td><pre>  CatalogingSupplyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>19</p></td>
<td><p>Invoicing instructions. Repeatable if two codes apply.</p>
<ul><li><em>01</em>&nbsp;&nbsp;Invoice this order separately</li>
<li><em>02</em>&nbsp;&nbsp;This order may be combined with others for invoicing purposes.</li>
<li><em>03</em>&nbsp;&nbsp;Invoice by fund account number</li>
<li><em>04</em>&nbsp;&nbsp;Invoice processing charges separately</li></ul></td>
<td><p>D</p></td>
<td><pre>InvoicingInstructionsCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>20</p></td>
<td><p>Expected terms: credit period</p></td>
<td><p>D</p></td>
<td><pre>PaymentTerms</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Number of days from date of invoice, or</p></td>
<td><p>D</p></td>
<td><pre>  NetDaysDue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Due date YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>  NetDueDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>21</p></td>
<td><p>% discount expected to apply to this order – decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>DiscountPercentage</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>22</p></td>
<td><p>Charge whole order to buyer’s pre-registered credit or charge card (empty element: absence means do not use card)</p></td>
<td><p>D</p></td>
<td><pre>ChargeToCard/</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Request detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Item detail</strong></p></td>
<td><p><strong>M</strong></p></td>
<td><p><strong>ItemDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Order request line number</p></td>
<td><p>M</p></td>
<td><pre>LineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>EAN-13 product number (mandatory unless trading partners have agreed to use an alternative product identifier)</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p><a name="AltProductId"></a>Alternative product identifier</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product ID type – use ONIX Code List 5</p></td>
<td><p>M</p></td>
<td><pre>  ProductIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = ‘01’ (proprietary)</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product number</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Item description</p></td>
<td><p>D</p></td>
<td><pre>ItemDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>“Bib number” – unique catalogue record number assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>  BibNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product form code – use ONIX code list 150</p></td>
<td><p>D</p></td>
<td><pre>  ProductForm</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Title: text</p></td>
<td><p>D</p></td>
<td><pre>  Title</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Author name (repeatable): text</p></td>
<td><p>D</p></td>
<td><pre>  Author</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Series title: text</p></td>
<td><p>D</p></td>
<td><pre>  SeriesTitle</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Volume or part designation: text</p></td>
<td><p>D</p></td>
<td><pre>  VolumeOrPart</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Edition statement: text</p></td>
<td><p>D</p></td>
<td><pre>  EditionStatement</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Place of publication: text</p></td>
<td><p>D</p></td>
<td><pre>  CityOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Country of publication: text</p></td>
<td><p>D</p></td>
<td><pre>  CountryOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publisher: text</p></td>
<td><p>D</p></td>
<td><pre>  PublisherName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Date of publication: YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>  DateOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Year of publication: YYYY</p></td>
<td><p>D</p></td>
<td><pre>  YearOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Quantity ordered</p></td>
<td><p>M</p></td>
<td><pre>OrderQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Order line references If included, must contain a reference number or a reference date or both.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>12</em>&nbsp;&nbsp;Buyer’s unique order line reference</li>
<li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>17</em>&nbsp;&nbsp;Promotion or deal reference</li>
<li><em>18</em>&nbsp;&nbsp;End customer order reference</li>
<li><em>24</em>&nbsp;&nbsp;Order source location reference</li>
<li><em>30</em>&nbsp;&nbsp;Supplier quotation line reference</li>
<li><em>31</em>&nbsp;&nbsp;Catalogue or price list reference</li>
<li><em>32</em>&nbsp;&nbsp;Authorisation for expense reference</li>
<li><em>33</em>&nbsp;&nbsp;Buyer’s internal supplier reference</li>
<li><em>34</em>&nbsp;&nbsp;Library new title list reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference number / string</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date-time</p>
(for format options see Header line 6)</td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Ship to / deliver to – for detail see Header line 12</p></td>
<td><p>D</p></td>
<td><pre>ShipToParty</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Order line qualifying dates</p></td>
<td><p>D</p></td>
<td><pre>DateCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Date YYYYMMDD</p></td>
<td><p>M</p></td>
<td><pre>  Date</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Date qualifier</p>
<li><em>01</em>&nbsp;&nbsp;Cancel if not shipped by date</li>
<li><em>02</em>&nbsp;&nbsp;Cancel if not shipped by date, unless not yet published</li>
<li><em>03</em>&nbsp;&nbsp;Fill all available by date</li>
<li><em>04</em>&nbsp;&nbsp;Do not ship before date</li></ul></td>
<td><p>M</p></td>
<td><pre>  DateQualifierCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Order line fill terms</p>
<ul><li><em>01</em>&nbsp;&nbsp;Fill all or kill all</li>
<li><em>02</em>&nbsp;&nbsp;Fill all or backorder all</li>
<li><em>03</em>&nbsp;&nbsp;Fill available, kill remainder</li>
<li><em>05</em>&nbsp;&nbsp;Fill available, backorder remainder and ship when complete</li>
<li><em>06</em>&nbsp;&nbsp;Fill available, backorder remainder and ship as available</li></ul></td>
<td><p>D</p></td>
<td><pre>FillTermsCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Expected unit price Price may be repeated if more than one price type or currency is to be included.</p></td>
<td><p>D</p></td>
<td><pre>Price</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier’s price identifier. Must be included if supplier has indicated (e.g. in a P&amp;A response) that the product is available at various identified prices according to the terms and conditions of supply. If both price amount and price identifier are specified, the buyer must ensure they are consistent. The price identifier must match a price identifier specified in the current ONIX record for the same item.</p></td>
<td><p>D</p></td>
<td><pre>  PriceIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary price identifier scheme – use ONIX code list 217</p></td>
<td><p>M</p></td>
<td><pre>    PriceIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary scheme name</p></td>
<td><p>D</p></td>
<td><pre>    IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier value</p></td>
<td><p>M</p></td>
<td><pre>    IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price amount</p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Currency: ISO 4217 currency code</p></td>
<td><p>D</p></td>
<td><pre>  CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Suggested retail price including tax</li>
<li><em>02</em>&nbsp;&nbsp;Suggested retail price excluding tax</li>
<li><em>03</em>&nbsp;&nbsp;Net price (unit cost) including tax</li>
<li><em>04</em>&nbsp;&nbsp;Net price (unit cost) excluding tax</li>
<li><em>05</em>&nbsp;&nbsp;Fixed retail price including tax</li>
<li><em>06</em>&nbsp;&nbsp;Fixed retail price excluding tax</li></ul></td>
<td><p>D</p></td>
<td><pre>  PriceQualifierCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price qualifier corresponding to the supplier’s price identifier – use ONIX code list 59.</p></td>
<td><p>D</p></td>
<td><pre>  PriceTypeQualifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Total % discount expected to apply to this order – decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>  DiscountPercentage</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Invoicing instructions</p>
<ul><li><em>04</em>&nbsp;&nbsp;Invoice processing charges separately</li>
<li><em>05</em>&nbsp;&nbsp;Invoice this order line separately</li></ul></td>
<td><p>D</p></td>
<td><pre>InvoicingInstructionsCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>All-copy detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>All-copy detail</strong></p></td>
<td><p>M</p></td>
<td><p><strong>ItemDetail.AllCopyDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Deliver to (when all copies of the item are to be delivered to the same location): a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>DeliverToLocation</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Destination location (when all copies of the item are to be shelved at the same location which is not the delivery location): a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>DestinationLocation</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Collection profile for all copies of the item: contains a library-defined code or descriptive text or both. May be repeated.</p></td>
<td><p>D</p></td>
<td><pre>CollectionProfile</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Collection code: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>  CollectionCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Collection description: text</p></td>
<td><p>D</p></td>
<td><pre>  CollectionDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Local call number, if the same for all copies of the item: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>LocalCallNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Classification</p></td>
<td><p>D</p></td>
<td><pre>Classification</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Classification scheme identifier:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>02</em>&nbsp;&nbsp;Dewey Decimal Classification</li>
<li><em>03</em>&nbsp;&nbsp;Abridged Dewey</li></ul></td>
<td><p>M</p></td>
<td><pre>  SubjectSchemeIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Classification scheme version number</p></td>
<td><p>D</p></td>
<td><pre>  SubjectSchemeVersion</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Classification code</p></td>
<td><p>M</p></td>
<td><pre>  SubjectCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Copy value, if the same for all copies. The replacement cost of an individual copy. A copy’s value need not be the same as its price.</p></td>
<td><p>D</p></td>
<td><pre>CopyValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Value amount</p></td>
<td><p>M</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Currency: ISO 4217 currency codes</p></td>
<td><p>D</p></td>
<td><pre>  CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Feature heading: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>FeatureHeading</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Filing suffix: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>FilingSuffix</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Loan status code. A code assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>LoanStatusCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Branch or other location code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>LocationCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Stock sequence or collection code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>StockSequenceCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Stock category code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>StockCategoryCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Reader interest code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>ReaderInterestCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Library rotation plan code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>LibraryRotationPlanCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>15</p></td>
<td><p>Size code: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>SizeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>16</p></td>
<td><p>Processing profile code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>ProcessingProfileCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>17</p></td>
<td><p>Special processing / servicing instruction (repeatable)</p>
<p>For values, see <a href="#Table3">Table 3</a></p></td>
<td><p>D</p></td>
<td><pre>ProcessingInstructionCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Supplier-applied copy number. Mandatory immediately following any instance of ProcessingInstructionCode whose code value is any of <em>AppliedCopyNumber</em>, <em>AppliedCopyNumberFrom</em> or <em>AppliedCopyNumberTo</em>.</p></td>
<td><p>D</p></td>
<td><pre>AppliedCopyNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>19</p></td>
<td><p>Spine label string. Mandatory immediately following any instance of ProcessingInstructionCode whose code value is SpineLabelString.</p></td>
<td><p>D</p></td>
<td><pre>SpineLabelString</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>20</p></td>
<td><p>Fund information (repeatable when the cost of the copies specified in the copy detail section is split across two or more funds)</p></td>
<td><p>D</p></td>
<td><pre>FundDetail</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Buyer’s fund/budget number: a string defined by the library</p></td>
<td><p>M</p></td>
<td><pre>  FundNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Buyer’s fund description: text</p></td>
<td><p>D</p></td>
<td><pre>  FundDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Percentage to charge to this fund: a decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>  Percent</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Amount to charge to this fund, in order currency: a decimal number<a href="#Note5">[5]</a></p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Buyer’s budget year</p></td>
<td><p>D</p></td>
<td><pre>  BudgetYear</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>21</p></td>
<td><p>Notes to supplier: text. To be used only by trading partner agreement. Not to be used for conveying machine-readable codes. Using this element will cause the order to be sidelined for manual processing.</p></td>
<td><p>D</p></td>
<td><pre>OrderNotes</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>22</p></td>
<td><p>Message required for all copies of this line item (absence of this element means “no message required”) (repeatable). The specified message is to be included in all documents relating to the order line.</p></td>
<td><p>D</p></td>
<td><pre>Message</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Message type: values 01 to 99 defined by trading partner agreement</p></td>
<td><p>M</p></td>
<td><pre>MessageType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Message content (repeatable)</p></td>
<td><p>M</p></td>
<td><pre>  MessageLine</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>23</p></td>
<td><p>Copy/ies specified in the copy detail section requested by (repeatable): text</p></td>
<td><p>D</p></td>
<td><pre>  RequestedBy</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>24</p></td>
<td><p>Copy/ies specified in the copy detail section approved by: text</p></td>
<td><p>D</p></td>
<td><pre>ApprovedBy</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Copy detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Copy detail</strong></p></td>
<td><p>D</p></td>
<td><p><strong>ItemDetail.CopyDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Sub-line number: a sequence number starting at 1 within each line item, and incremented for each repeat of the copy detail section.</p></td>
<td><p>M</p></td>
<td><pre>SubLineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Number of copies: integer</p></td>
<td><p>M</p></td>
<td><pre>CopyQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Copy number assigned by the library. If used, the number of occurrences of this element must equal the quantity in the preceding element, to give a unique copy number for each copy in the copy detail section.</p></td>
<td><p>D</p></td>
<td><pre>CopyNumber</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Deliver to (when the copies specified in the copy detail section are to be delivered to a different location): a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>DeliverToLocation</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Destination location (when the copies specified in the copy detail section are to be shelved at a location which is not the delivery location): a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>DestinationLocation</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Collection profile: contains a library-defined code or descriptive text or both. May be repeated.</p></td>
<td><p>D</p></td>
<td><pre>CollectionProfile</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Collection code: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>  CollectionCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Collection description: text</p></td>
<td><p>D</p></td>
<td><pre>  CollectionDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Local call number: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>LocalCallNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Classification</p></td>
<td><p>D</p></td>
<td><pre>Classification</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Classification scheme identifier:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>02</em>&nbsp;&nbsp;Dewey Decimal Classification</li>
<li><em>03</em>&nbsp;&nbsp;Abridged Dewey</li></ul></td>
<td><p>M</p></td>
<td><pre>  SubjectSchemeIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Classification scheme version number</p></td>
<td><p>D</p></td>
<td><pre>  SubjectSchemeVersion</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Classification code</p></td>
<td><p>M</p></td>
<td><pre>  SubjectCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Copy value. The replacement cost of an individual copy. A copy’s value need not be the same as its price, nor the same as for other copies of the same item.</p></td>
<td><p>D</p></td>
<td><pre>CopyValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Value amount</p></td>
<td><p>M</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Currency: ISO 4217 currency codes</p></td>
<td><p>D</p></td>
<td><pre>  CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Feature heading: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>FeatureHeading</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Filing suffix: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>FilingSuffix</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Loan status code. A code assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>LoanStatusCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Branch or other location code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>LocationCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Stock sequence or collection code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>StockSequenceCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>15</p></td>
<td><p>Stock category code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>StockCategoryCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>16</p></td>
<td><p>Reader interest code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>ReaderInterestCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>17</p></td>
<td><p>Library rotation plan code, assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>LibraryRotationPlanCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Size code: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>SizeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>19</p></td>
<td><p>Processing profile code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>ProcessingProfileCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>20</p></td>
<td><p>Special processing / servicing instruction (repeatable)</p>
For values, see <a href="#Table3">Table 3</a></td>
<td><p>D</p></td>
<td><pre>ProcessingInstructionCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>21</p></td>
<td><p>Supplier-applied copy number. Mandatory immediately following any instance of ProcessingInstructionCode whose code value is any of <em>AppliedCopyNumber</em>, <em>AppliedCopyNumberFrom</em> or <em>AppliedCopyNumberTo</em>.</p></td>
<td><p>D</p></td>
<td><pre>AppliedCopyNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>22</p></td>
<td><p>Spine label string. Mandatory immediately following any instance of ProcessingInstructionCode whose code value is <em>SpineLabelString</em>.</p></td>
<td><p>D</p></td>
<td><pre>SpineLabelString</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>23</p></td>
<td><p>Fund information (repeatable when the cost of the copies specified in the copy detail section is split across two or more funds)</p></td>
<td><p>D</p></td>
<td><pre>FundDetail</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Buyer’s fund/budget number: a string defined by the library</p></td>
<td><p>M</p></td>
<td><pre>  FundNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Buyer’s fund description: text</p></td>
<td><p>D</p></td>
<td><pre>FundDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Percentage to charge to this fund: a decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>  Percent</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Amount to charge to this fund, in order currency: a decimal number[6]</p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Buyer’s budget year</p></td>
<td><p>D</p></td>
<td><pre>BudgetYear</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>24</p></td>
<td><p>Notes to supplier: text. To be used only by trading partner agreement. Not to be used for conveying machine-readable codes. Using this element will cause the order to be sidelined for manual processing.</p></td>
<td><p>D</p></td>
<td><pre>OrderNotes</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>25</p></td>
<td><p>Message required at copy level (absence of this element means “no message required”) (repeatable). The specified message is to be included in all documents relating to this copy.</p></td>
<td><p>D</p></td>
<td><pre>Message</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Message type: values 01 to 99 defined by trading partner agreement</p></td>
<td><p>M</p></td>
<td><pre>  MessageType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Message content (repeatable)</p></td>
<td><p>M</p></td>
<td><pre>  MessageLine</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>26</p></td>
<td><p>Copy/ies specified in the copy detail section requested by (repeatable): text</p></td>
<td><p>D</p></td>
<td><pre>RequestedBy</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>27</p></td>
<td><p>Copy/ies specified in the copy detail section approved by: text</p></td>
<td><p>D</p></td>
<td><pre>ApprovedBy</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of an Order Request XML payload using either the SOAP or the
HTTPS protocol and the POST method:*

```
<OrderRequest version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/Order">
  <Header>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
    <RequestNumber>001</RequestNumber>
    <OrderNumber>1012345</OrderNumber>
    <IssueDateTime>20180520T1525</IssueDateTime>
  </Header>
  <ItemDetail>
    <LineNumber>1</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780123456789</IDValue>
    </ProductIdentifier>
    <OrderQuantity>5</OrderQuantity>
    <Price>
      <MonetaryAmount>9.99</MonetaryAmount>
      <PriceQualifierCode>05</PriceQualifierCode>
    </Price>
    <AllCopyDetail>
      <ProcessingProfileCode>A1</ProcessingProfileCode>
    </AllCopyDetail>
    <CopyDetail>
      <SubLineNumber>1</SubLineNumber>
      <CopyQuantity>1</CopyQuantity>
      <DeliverToLocation>A</DeliverToLocation>
    </CopyDetail>
    <CopyDetail>
      <SubLineNumber>2</SubLineNumber>
      <CopyQuantity>1</CopyQuantity>
      <DeliverToLocation>B</DeliverToLocation>
    </CopyDetail>
    <CopyDetail>
      <SubLineNumber>3</SubLineNumber>
      <CopyQuantity>1</CopyQuantity>
      <DeliverToLocation>C</DeliverToLocation>
    </CopyDetail>
    <CopyDetail>
      <SubLineNumber>4</SubLineNumber>
      <CopyQuantity>1</CopyQuantity>
      <DeliverToLocation>D</DeliverToLocation>
    </CopyDetail>
    <CopyDetail>
      <SubLineNumber>5</SubLineNumber>
      <CopyQuantity>1</CopyQuantity>
      <DeliverToLocation>E</DeliverToLocation>
    </CopyDetail>
  </ItemDetail>
  <ItemDetail>
    <LineNumber>2</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780987654321</IDValue>
    </ProductIdentifier>
    <OrderQuantity>1</OrderQuantity>
    <Price>
      <MonetaryAmount>15.99</MonetaryAmount>
      <PriceQualifierCode>05</PriceQualifierCode>
    </Price>
    <AllCopyDetail>
      <ProcessingProfileCode>A2</ProcessingProfileCode>
      <DeliverToLocation>A</DeliverToLocation>
    </AllCopyDetail>
  </ItemDetail>
</OrderRequest>
```

*Example of an Order Request JSON payload using either the SOAP or the
HTTPS protocol and the POST method:*

```
{"OrderRequest": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/Order",
  "Header": {
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "OrderNumber": "1012345",
    "IssueDateTime": "20180520T1525"
  },
  "ItemDetail": [
    {
      "LineNumber": 1,
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780123456789"
      },
      "OrderQuantity": 5,
      "Price": {
        "MonetaryAmount": 9.99,
        "PriceQualifierCode": "05"
      },
      "AllCopyDetail": {"ProcessingProfileCode": "A1"},
      "CopyDetail": [
        {
          "SubLineNumber": 1,
          "CopyQuantity": 1,
          "DeliverToLocation": "A"
        },
        {
          "SubLineNumber": 2,
          "CopyQuantity": 1,
          "DeliverToLocation": "B"
        },
        {
          "SubLineNumber": 3,
          "CopyQuantity": 1,
          "DeliverToLocation": "C"
        },
        {
          "SubLineNumber": 4,
          "CopyQuantity": 1,
          "DeliverToLocation": "D"
        },
        {
          "SubLineNumber": 5,
          "CopyQuantity": 1,
          "DeliverToLocation": "E"
        }
      ]
    },
    {
      "LineNumber": 2,
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780987654321"
      },
      "OrderQuantity": 1,
      "Price": {
        "MonetaryAmount": 15.99,
        "PriceQualifierCode": "05"
      },
      "AllCopyDetail": {
        "ProcessingProfileCode": "A2",
        "DeliverToLocation": "A"
      }
    }
  ]
}}
```

### RESPONSE

The Response will use the protocol corresponding to the Request. If the
Request uses the basic HTTPS protocol, the Response will be an XML or
JSON document as specified below attached to a normal HTTPS header. If
the Request uses the SOAP protocol, the Response will contain a SOAP
response message whose body will contain the XML or JSON document
specified below.

**XML document encoding begins:** `<OrderResponse version="0.9">...`

**JSON document encoding begins:** `{ "OrderResponse": { "version": "0.9"...`

<table>
<tbody>
<tr valign="top">
  <th></th>
  <th>Description</th>
  <th><a href="#Notes2">[2]</a></th>
  <th>XML tag / JSON field name</th>
  <th><a href="#Notes3">[3]</a></th>
</tr>
<tr valign="top">
<td colspan="5"><h4>Header</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Response payload header</strong></p></td>
<td><p>M</p></td>
<td><p><strong>Header</strong></p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Document date/time: the date/time when the report was generated. Permitted formats are:</p>
<ul><li>YYYYMMDD</li>
<li>YYYYMMDDTHHMM</li>
<li>YYYYMMDDTHHMMZ (universal time)</li>
<li>YYYYMMDDTHHMM±HHMM (time zone)</li></ul>
<p>where “T” represents itself, ie letter T</p></td>
<td><p>M</p></td>
<td><pre>IssueDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Sender (web service host)</p></td>
<td><p>M</p></td>
<td><pre>SenderIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Sender ID type - see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>  SenderIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Identification number / string of this response</p></td>
<td><p>D</p></td>
<td><pre>ResponseNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Account identifier. Mandatory if included in request.</p></td>
<td><p>D</p></td>
<td><pre>AccountIdentifier.</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier. Must be specified if an account identifier is specified. Permitted schemes are:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li>
<li><em>11</em>&nbsp;&nbsp;PubEasy PIN</li></ul></td>
<td><p>M</p></td>
<td><pre>  AccountIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>References: the order number must be quoted; the request number, date/time of request and other order references must be quoted if included in the request.</p></td>
<td><p>M</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Number or date/time of associated request</li>
<li><em>11</em>&nbsp;&nbsp;Order number specified in the request</li>
<li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>17</em>&nbsp;&nbsp;Promotion or deal reference</li>
<li><em>24</em>&nbsp;&nbsp;Order source location reference</li>
<li><em>29</em>&nbsp;&nbsp;Supplier’s quotation reference</li>
<li><em>32</em>&nbsp;&nbsp;Authorisation for expense reference</li>
<li><em>35</em>&nbsp;&nbsp;Library’s supplier reference</li>
<li><em>36</em>&nbsp;&nbsp;Supplier’s library customer reference</li>
<li><em>37</em>&nbsp;&nbsp;Library’s additional order reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference number / string</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date or date and time. Mandatory if an IssueDateTime is included in the request (for format options see line 1).</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Response function – normally only sent if the same order request is repeated.</p>
<ul><li><em>01</em>&nbsp;&nbsp;Response sent for the first time (default)</li>
<li><em>02</em>&nbsp;&nbsp;Duplicate response to a repeated order request><a href="#Note7">[7]</a></li></ul></td>
<td><p>D</p></td>
<td><pre>ResponsePurposeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Response code, if there are exception conditions, in which case this composite terminates the response.</p></td>
<td><p>D</p></td>
<td><pre>ResponseCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Response type code. Suggested code values:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Service unavailable</li>
<li><em>02</em>&nbsp;&nbsp;Invalid ClientID or ClientPassword</li>
<li><em>03</em>&nbsp;&nbsp;Server unable to process request – a reason should normally be given as a free text description – see below</li>
<li><em>10</em>&nbsp;&nbsp;Duplicate order number</li>
<li><em>16</em>&nbsp;&nbsp;Invalid or unknown account or supplier identifier</li>
<li><em>19</em>&nbsp;&nbsp;Server unable to process request – unable to contact supplier</li>
<li><em>20</em>&nbsp;&nbsp;Order request acknowledged – awaiting response from supplier</li></ul></td>
<td><p>M</p></td>
<td><pre>  ResponseType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Free text description / reason for response</p></td>
<td><p>D</p></td>
<td><pre>  ResponseTypeDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Supplier identifier (only included if specified in the request header; mandatory if the response type code is ‘19’ or ‘20’).</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier ID type - see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>  SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Order type code</p>
<ul><li><em>01</em>&nbsp;&nbsp;New order (default)</li>
<li><em>02</em>&nbsp;&nbsp;Order for approval or inspection copies</li>
<li><em>03</em>&nbsp;&nbsp;Confirmation order (confirming an order placed on a supplier’s website)</li></ul></td>
<td><p>D</p></td>
<td><pre>OrderTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Default currency of values given in the response. If omitted, ‘GBP’ is assumed.</p></td>
<td><p>D</p></td>
<td><pre>CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Shipping detail <em>(if supplier has more than one warehouse, the default warehouse location from which goods will be shipped; may be overridden at the item detail level – see response detail line 12)</em></p></td>
<td><p>D</p></td>
<td><pre>ShippingFrom</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location must contain at least one location identifier or one location name or both.</p></td>
<td><p>M</p></td>
<td><pre>  Location</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location identifier</p></td>
<td><p>D</p></td>
<td><pre>    LocationIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location ID type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</p></li></ul></td>
<td><p>M</p></td>
<td><pre>      LocationIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary location ID type name</p></td>
<td><p>D</p></td>
<td><pre>      IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location ID value</p></td>
<td><p>M</p></td>
<td><pre>      IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location name</p></td>
<td><p>D</p></td>
<td><pre>    LocationName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Order status – mandatory unless there is an exception condition.</p>
<ul><li><em>01</em>&nbsp;&nbsp;Order accepted – shipping</li>
<li><em>02</em>&nbsp;&nbsp;Order accepted – all backordered</li>
<li><em>03</em>&nbsp;&nbsp;Order accepted – part shipping, part backordered – see detail</li>
<li><em>04</em>&nbsp;&nbsp;Order accepted but cannot be processed normally – see order status message</li>
<li><em>05</em>&nbsp;&nbsp;Order not accepted – see detail</li></ul></td>
<td><p>D</p></td>
<td><pre>OrderStatus</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Order status message. Mandatory if the order status is ‘04’.</p></td>
<td><p>D</p></td>
<td><pre>OrderStatusMessage</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Response detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Responses for all items included in the order request. Mandatory unless the response type code indicates exception conditions.</strong></p></td>
<td><p>D</p></td>
<td><p>ItemDetail</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Line item number</p></td>
<td><p>M</p></td>
<td><pre>LineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>EAN-13 product number (mandatory unless trading partners have agreed to use an alternative product identifier)</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Alternative product identifier – for details see the <a href="#AltProductId">corresponding line</a> in Request item detail</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Item description</p></td>
<td><p>D</p></td>
<td><pre>ItemDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td>“Bib number” – unique catalogue record number assigned by the library</td>
<td><p>D</p></td>
<td><pre>  BibNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product form code – use ONIX code list 150</p></td>
<td><p>D</p></td>
<td><pre>  ProductForm</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Title: text</p></td>
<td><p>D</p></td>
<td><pre>  Title</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Author name (repeatable): text</p></td>
<td><p>D</p></td>
<td><pre>  Author</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Series title: text</p></td>
<td><p>D</p></td>
<td><pre>  SeriesTitle</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Volume or part designation: text</p></td>
<td><p>D</p></td>
<td><pre>  VolumeOrPart</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Edition statement: text</p></td>
<td><p>D</p></td>
<td><pre>  EditionStatement</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Place of publication: text</p></td>
<td><p>D</p></td>
<td><pre>  CityOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Country of publication: text</p></td>
<td><p>D</p></td>
<td><pre>  CountryOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publisher: text</p></td>
<td><p>D</p></td>
<td><pre>  PublisherName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Date of publication: YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>  DateOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Year of publication: YYYY</p></td>
<td><p>D</p></td>
<td><pre>  YearOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Quantity ordered</p></td>
<td><p>M</p></td>
<td><pre>OrderQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Order line references If included, must contain a reference number or a reference date or both.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>12</em>&nbsp;&nbsp;Buyer’s unique order line reference</li>
<li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>17</em>&nbsp;&nbsp;Promotion or deal reference</li>
<li><em>18</em>&nbsp;&nbsp;End customer order reference</li>
<li><em>24</em>&nbsp;&nbsp;Order source location reference</li>
<li><em>30</em>&nbsp;&nbsp;Supplier quotation line reference</li>
<li><em>31</em>&nbsp;&nbsp;Catalogue or price list reference</li>
<li><em>32</em>&nbsp;&nbsp;Authorisation for expense reference</li>
<li><em>33</em>&nbsp;&nbsp;Buyer’s internal supplier reference</li>
<li><em>34</em>&nbsp;&nbsp;Library new title list reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference number / string</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date-time</p>
(for format options see Header line 1)</td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>For digital items, the technical protection method(s) applied as a condition of sale at the supplier’s price identifier. Repeatable – use ONIX code list 144.</p></td>
<td><p>D</p></td>
<td><pre>EpubTechnicalProtection</pre></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Usage constraint(s) associated with the supplier’s identified price. Repeatable</p></td>
<td><p>D</p></td>
<td><pre>PriceConstraint</pre></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of constraint – use ONIX code list 230.</p></td>
<td><p>M</p></td>
<td><pre>  PriceConstraintType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Status of constraint – use ONIX code list 146.</p></td>
<td><p>M</p></td>
<td><pre>  PriceConstraintStatus</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Limit of constraint</p></td>
<td><p>D</p></td>
<td><pre>  PriceConstraintLimit</pre></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Limiting quantity</p></td>
<td><p>M</p></td>
<td><pre>    Quantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity unit – use ONIX code list 147.</p></td>
<td><p>M</p></td>
<td><pre>    PriceConstraintUnit</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>For digital items, the licensing terms applicable as a condition of sale at the supplier’s identified price.</p></td>
<td><p>D</p></td>
<td><pre>EpubLicense</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>License name</p></td>
<td><p>M</p></td>
<td><pre>  EpubLicenseName</pre></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>License expression</p></td>
<td><p>D</p></td>
<td><pre>  EpubLicenseExpression</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Expression type – use ONIX code list 218.</p></td>
<td><p>M</p></td>
<td><pre>    EpubLicenseExpressionType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Expression type name</p></td>
<td><p>D</p></td>
<td><pre>    EpubLicenseExpressionTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>URI of license expression</p></td>
<td><p>M</p></td>
<td><pre>    EpubLicenseExpressionLink</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Further conditions applicable to the supplier’s identified price.</p></td>
<td><p>D</p></td>
<td><pre>PriceCondition</pre></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of condition – use ONIX code list 167.</p></td>
<td><p>M</p></td>
<td><p>  PriceConditionType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity associated with condition</p></td>
<td><p>D</p></td>
<td><p>  PriceConditionQuantity</p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of quantity – use ONIX code list 168.</p></td>
<td><p>M</p></td>
<td><p>    PriceConditionQuantityType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity</p></td>
<td><p>M</p></td>
<td><p>    Quantity</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity unit – use ONIX code list 169.</p></td>
<td><p>M</p></td>
<td><pre>    QuantityUnit</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Offered or confirmed unit price Price may be repeated if more than one price type or currency is to be included.</p></td>
<td><p>D</p></td>
<td><pre>Price</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier’s price identifier, associating the price with a set of terms and conditions which may be confirmed by the immediate following elements. The price identifier must match a price identifier specified in the current ONIX record for the same item.</p></td>
<td><p>D</p></td>
<td><pre>  PriceIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary price identifier scheme – use ONIX code list 217</p></td>
<td><p>M</p></td>
<td><pre>    PriceIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary scheme name</p></td>
<td><p>D</p></td>
<td><pre>    IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier value</p></td>
<td><p>M</p></td>
<td><pre>    IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price amount</p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Currency: ISO 4217 currency code</p></td>
<td><p>D</p></td>
<td><pre>  CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Suggested retail price including tax</li>
<li><em>02</em>&nbsp;&nbsp;Suggested retail price excluding tax</li>
<li><em>03</em>&nbsp;&nbsp;Net price (unit cost) including tax</li>
<li><em>04</em>&nbsp;&nbsp;Net price (unit cost) excluding tax</li>
<li><em>05</em>&nbsp;&nbsp;Fixed retail price including tax</li>
<li><em>06</em>&nbsp;&nbsp;Fixed retail price excluding tax</li></ul></td>
<td><p>D</p></td>
<td><pre>  PriceQualifierCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price qualifier corresponding to the supplier’s price identifier – use ONIX code list 59.</p></td>
<td><p>D</p></td>
<td><pre>  PriceTypeQualifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Total % discount expected to apply to this price – decimal number between 0 and 100</p>
</td>
<td><p>D</p></td>
<td><pre>  DiscountPercentage</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Order line response status code. Mandatory for each order line. See <a href="#Table1">Table 1</a> for EDItEUR order line status codes that may be used in this context.</p></td>
<td><p>M</p></td>
<td><pre>OrderLineStatusCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Status code scheme</p>
<ul><li><em>01</em>&nbsp;&nbsp;Sender’s own scheme</li>
<li><em>02</em>&nbsp;&nbsp;Scheme maintained by EDItEUR</li></ul></td>
<td><p>M</p></td>
<td><pre>  StatusCodeType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Status code</p></td>
<td><p>M</p></td>
<td><pre>  StatusCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Quantity in process for shipping now</p></td>
<td><p>D</p></td>
<td><pre>QuantityShipping</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>15</td>
<td><p>Shipping detail (used only if the shipment will be from a location other than the default specified in the header)</p></td>
<td><p>D</p></td>
<td><pre>ShippingFrom</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location – must include at least one identifier, a name, or both</p></td>
<td><p>M</p></td>
<td><pre>  Location</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location identifier</p></td>
<td><p>D</p></td>
<td><pre>    LocationIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location ID type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li></ul></td>
<td><p>M</p></td>
<td><pre>      LocationIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary ID type name</p></td>
<td><p>D</p></td>
<td><pre>      IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location ID value</p></td>
<td><p>M</p></td>
<td><pre>      IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location name</p></td>
<td><p>D</p></td>
<td><pre>    LocationName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>16</p></td>
<td><p>Quantity backordered</p></td>
<td><p>D</p></td>
<td><pre>BackorderedQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>17</p></td>
<td><p>Quantity cancelled</p></td>
<td><p>D</p></td>
<td><pre>CanceledQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Product availability status</p></td>
<td><p>D</p></td>
<td><pre>AvailabilityCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publisher product availability code value. Should be included if known, unless the order line is shipping now. See Table 2 on page 24 for ONIX product availability status codes that may be used in this context.</p></td>
<td><p>D</p></td>
<td><pre>  PublisherAvailabilityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Availability date for the ordered product (YYYYMMDD) – mandatory with certain publisher product availability code values.</p></td>
<td><p>D</p></td>
<td><pre>  ExpectedShipDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publishing status code value – see ONIX codelist 64</p></td>
<td><p>D</p></td>
<td><pre>  PublishingStatusCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>On-display date for the ordered product (YYYYMMDD) – included if display is embargoed until the specified date, or if mandated by a publishing status code value.</p></td>
<td><p>D</p></td>
<td><pre>  LibraryOnDisplayDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Expected order time for a non-stock item – in days</p></td>
<td><p>D</p></td>
<td><pre>  OrderTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>23</td>
<td><p>Substitute product details <em>(for further description see corresponding details for the ordered product)</em>.</p></td>
<td><p>D</p></td>
<td><pre>Substitute</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>EAN-13 product number</p></td>
<td><p>D</p></td>
<td><pre>  EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Alternative product identifier - see <a href="#AltProductId">above</a>.</p></td>
<td><p>D</p></td>
<td><pre>  ProductIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>24</p></td>
<td><p>Message required for all or specific copies of this line item (absence of this element means “no message required”) (repeatable). The specified message is to be included in all documents relating to the order line.</p></td>
<td><p>D</p></td>
<td><pre>Message</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Sub-line number of specific copy to which the message relates. Must match a sub-line number for this line item specified in the request. If omitted, the message relates to the order line as a whole.</p></td>
<td><p>D</p></td>
<td><pre>  SubLineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Message type: values 01 to 99 defined by trading partner agreement</p></td>
<td><p>M</p></td>
<td><pre>  MessageType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Message content (repeatable)</p></td>
<td><p>M</p></td>
<td><pre>  MessageLine</pre></td>
<td><p>R</p></td>
</tr>
</tbody>
</table>

*Example of an Order Response XML payload:*

```
<OrderResponse version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/Order">
  <Header>
    <IssueDateTime>20180520T1526</IssueDateTime>
    <SenderIdentifier>
      <SenderIDType>01</SenderIDType>
      <IDValue>XYZ</IDValue>
    </SenderIdentifier>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
    <ReferenceCoded>
      <ReferenceTypeCode>01</ReferenceTypeCode>
      <ReferenceNumber>001</ReferenceNumber>
      <ReferenceDateTime>20180520T1525</ReferenceDateTime>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>1012345</ReferenceNumber>
    </ReferenceCoded>
    <OrderStatus>03</OrderStatus>
  </Header>
  <ItemDetail>
    <LineNumber>1</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780123456789</IDValue>
    </ProductIdentifier>
    <OrderQuantity>5</OrderQuantity>
    <ReferenceCoded>
      <ReferenceTypeCode>12</ReferenceTypeCode>
      <ReferenceNumber>1</ReferenceNumber>
    </ReferenceCoded>
    <Price>
      <MonetaryAmount>9.99</MonetaryAmount>
      <PriceQualifierCode>05</PriceQualifierCode>
    </Price>
    <OrderLineStatusCoded>
      <StatusCodeType>02</StatusCodeType>
      <StatusCode>AcceptedShipping</StatusCode>
    </OrderLineStatusCoded>
    <QuantityShipping>5</QuantityShipping>
  </ItemDetail>
  <ItemDetail>
   	<LineNumber>2</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780987654321</IDValue>
    </ProductIdentifier>
    <OrderQuantity>1</OrderQuantity>
    <ReferenceCoded>
      <ReferenceTypeCode>12</ReferenceTypeCode>
      <ReferenceNumber>2</ReferenceNumber>
    </ReferenceCoded>
    <Price>
      <MonetaryAmount>15.99</MonetaryAmount>
      <PriceQualifierCode>05</PriceQualifierCode>
    </Price>
    <OrderLineStatusCoded>
      <StatusCodeType>02</StatusCodeType>
      <StatusCode>AcceptedBackordered</StatusCode>
    </OrderLineStatusCoded>
    <BackorderedQuantity>1</BackorderedQuantity>
    <PublisherAvailabilityCode>31</PublisherAvailabilityCode>
    <ExpectedShipDate>20180601</ExpectedShipDate>
</ItemDetail>
</OrderResponse>
```

*See page 26 for an example of a PriceIdentifier fragment for a digital
item supplied for lending under specified conditions.*

*Example of an Order Response JSON payload:*

```
{"OrderResponse": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/Order",
  "Header": {
    "IssueDateTime": "20180520T1526",
    "SenderIdentifier": {
      "SenderIDType": "01",
      "IDValue": "XYZ"
    },
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "ReferenceCoded": [
      {
        "ReferenceTypeCode": "01",
        "ReferenceNumber": "001",
        "ReferenceDateTime": "20180520T1525"
      },
      {
        "ReferenceTypeCode": "11",
        "ReferenceNumber": "1012345"
      }
    ],
    "OrderStatus": "03"
  },
  "ItemDetail": [
    {
      "LineNumber": 1,
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780123456789"
      },
      "OrderQuantity": 5,
      "ReferenceCoded": {
        "ReferenceTypeCode": "12",
        "ReferenceNumber": 1
      },
      "Price": {
        "MonetaryAmount": 9.99,
        "PriceQualifierCode": "05"
      },
      "OrderLineStatusCoded": {
        "StatusCodeType": "02",
        "StatusCode": "AcceptedShipping"
      },
      "QuantityShipping": 5
    },
    {
      "LineNumber": "2",
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780987654321"
      },
      "OrderQuantity": 1,
      "ReferenceCoded": {
        "ReferenceTypeCode": "12",
        "ReferenceNumber": 2
      },
      "Price": {
        "MonetaryAmount": 15.99,
        "PriceQualifierCode": "05"
      },
      "OrderLineStatusCoded": {
        "StatusCodeType": "02",
        "StatusCode": "AcceptedBackordered"
      },
      "BackorderedQuantity": 1,
	  "AvailabilityCoded": {
        "PublisherAvailabilityCode": "31",
        "ExpectedShipDate": "20180601"
      }
    }
  ]
}}
```

**Table 1<a name="Table1"></a>: EDItEUR order line status codes**

Used in OrderLineStatusCoded.StatusCode, when the
StatusCodeType is specified as ‘02’ (EDItEUR). Code values followed by
an asterisk \* have been defined by BIC for this standard and are to be
proposed as additions to the EDItEUR code list. The equivalent EDIFACT
code (List 12B) is shown for information only, as the ONIX code must
always be used.

<table>
<tbody>
<tr valign="top">
<td><p><strong>No</strong></p></td>
<td><p><strong>Value</strong></p></td>
<td><p><strong>Description</strong></p></td>
<td><p><strong>Equivalent<br />
EDIFACT code<br />
(List 12B)</strong></p></td>
</tr>
<tr valign="top">
<td>1</td>
<td><em>AcceptedBackordered</em></td>
<td><p>Order line accepted as backorder (“due”) (Order Response only)</p></td>
<td><p>400</p></td>
</tr>
<tr valign="top">
<td>2</td>
<td><em>AcceptedPartShippingPartBackordered</em></td>
<td><p>Order line accepted, part in process for immediate shipping, part backordered (Order Response only)</p></td>
<td></td>
</tr>
<tr valign="top">
<td>3</td>
<td><em>AcceptedPartShippingPartCanceled</em></td>
<td><p>Order line accepted, part in process for immediate shipping, part cancelled (Order Response only)</p></td>
<td></td>
</tr>
<tr valign="top">
<td>4</td>
<td><em>AcceptedShipping</em></td>
<td><p>Order line accepted, all in process for immediate shipping (Order Response only)</p></td>
<td><p>100</p></td>
</tr>
<tr valign="top">
<td>5</td>
<td><em>AlreadyShipped</em></td>
<td><p>Order line already shipped or in process for shipping</p></td>
<td><p>800</p></td>
</tr>
<tr valign="top">
<td>6</td>
<td><em>BackorderedAwaitingMinimumOrder</em></td>
<td><p>Backordered until minimum order quantity or value is reached</p></td>
<td><p>103 / 403</p></td>
</tr>
<tr valign="top">
<td>7</td>
<td><em>BackorderedAwaitingReceipt</em></td>
<td><p>Backordered and has been despatched from our supplier, awaiting receipt</p></td>
<td><p>404</p></td>
</tr>
<tr valign="top">
<td>8</td>
<td><em>BackorderedAwaitingSupply</em></td>
<td><p>Backordered – awaiting supply</p></td>
<td><p>400</p></td>
</tr>
<tr valign="top">
<td>9</td>
<td><em>BackorderedChasingSupplier</em></td>
<td><p>Order line has been chased to our supplier</p></td>
<td><p>413</p></td>
</tr>
<tr valign="top">
<td>10</td>
<td><em>BackorderedDateChange</em></td>
<td><p>Backordered, note change to expected availability date</p></td>
<td></td>
</tr>
<tr valign="top">
<td>11</td>
<td><em>BackorderedOnOrderFromOverseas</em></td>
<td><p>Backordered – has been ordered from overseas supplier</p></td>
<td><p>402</p></td>
</tr>
<tr valign="top">
<td>12</td>
<td><em>BackorderedPriceChange</em></td>
<td><p>Backordered, note price change</p></td>
<td></td>
</tr>
<tr valign="top">
<td>13</td>
<td><em>BackorderedTitleChange</em></td>
<td><p>Backordered, note title change (NYP only)</p></td>
<td></td>
</tr>
<tr valign="top">
<td>14</td>
<td><em>BackorderedStockTaking*</em></td>
<td><p>Backordered – currently unavailable due to stock taking</p></td>
<td></td>
</tr>
<tr valign="top">
<td>15</td>
<td><em>CanceledByBuyer</em></td>
<td><p>Order line cancelled at buyer’s request</p></td>
<td><p>222</p></td>
</tr>
<tr valign="top">
<td>16</td>
<td><em>CanceledCannotSupply</em></td>
<td><p>Order line cancelled, item cannot be supplied (eg out of print)</p></td>
<td><p>223</p></td>
</tr>
<tr valign="top">
<td>17</td>
<td><em>CanceledDiscountQuery</em></td>
<td><p>Discount query: order line cancelled</p></td>
<td><p>202</p></td>
</tr>
<tr valign="top">
<td>18</td>
<td><em>CanceledDuplicateOrder</em></td>
<td><p>Order appears to be a duplicate: order line cancelled</p></td>
<td><p>208</p></td>
</tr>
<tr valign="top">
<td>19</td>
<td><em>CanceledInvalid</em></td>
<td><p>Ordered item cannot be recognized</p></td>
<td></td>
</tr>
<tr valign="top">
<td>20</td>
<td><em>CanceledMinimumOrderReq</em></td>
<td><p>Minimum order value required: order line cancelled</p></td>
<td><p>203</p></td>
</tr>
<tr valign="top">
<td>21</td>
<td><em>CanceledOutOfTime</em></td>
<td><p>Order line cancelled, past cancelation date</p></td>
<td><p>221</p></td>
</tr>
<tr valign="top">
<td>22</td>
<td><em>CanceledPriceQuery</em></td>
<td><p>Price query: order line cancelled</p></td>
<td><p>201</p></td>
</tr>
<tr valign="top">
<td>23</td>
<td><em>CanceledPriceIdentifierMismatch*</em></td>
<td><p>Price identifier invalid or doesn’t match price amount: order line cancelled</p></td>
<td></td>
</tr>
<tr valign="top">
<td>24</td>
<td><em>CanceledPromotionInvalid</em></td>
<td><p>Promotional deal invalid or expired: order line cancelled</p></td>
<td><p>205 / 206</p></td>
</tr>
<tr valign="top">
<td>25</td>
<td><em>CanceledSubstOffered</em></td>
<td><p>Order line cancelled, substitute product is offered for separate order</p></td>
<td><p>210</p></td>
</tr>
<tr valign="top">
<td>26</td>
<td><em>CanceledTryOtherLocation</em></td>
<td><p>Order line cancelled, but could be supplied at a Ship From location other than that specified by the buyer: must be accompanied by the identity of the other location</p></td>
<td></td>
</tr>
<tr valign="top">
<td>27</td>
<td><em>CanceledUnknown*</em></td>
<td><p>Order line cancelled – unknown product or source of supply</p></td>
<td></td>
</tr>
<tr valign="top">
<td>28</td>
<td><em>CanceledRightsRestricted*</em></td>
<td><p>Order line cancelled –cannot supply to customer due to rights restrictions</p></td>
<td></td>
</tr>
<tr valign="top">
<td>29</td>
<td><em>CanceledCannotShipByRequestedDate*</em></td>
<td><p>Order line cancelled – cannot ship by requested “ship by” date</p></td>
<td></td>
</tr>
<tr valign="top">
<td>30</td>
<td><em>CanceledSold*</em></td>
<td><p>Order line cancelled – second-hand or antiquarian item already sold</p></td>
<td></td>
</tr>
<tr valign="top">
<td>31</td>
<td><em>CanceledStockTaking*</em></td>
<td><p>Order line cancelled – currently unavailable due to stock taking</p></td>
<td></td>
</tr>
<tr valign="top">
<td>32</td>
<td><em>HeldAccountStopped</em></td>
<td><p>Buyer’s account is temporarily stopped: product availability is reported as usual, but order will not be released until account is cleared</p></td>
<td><p>500</p></td>
</tr>
<tr valign="top">
<td>33</td>
<td><em>HeldAwaitingBuyerInstruction</em></td>
<td><p>Awaiting instruction from buyer: order line held</p></td>
<td><p>412</p></td>
</tr>
<tr valign="top">
<td>34</td>
<td><em>HeldDiscountQuery</em></td>
<td><p>Discount query: order line held awaiting customer response</p></td>
<td><p>102</p></td>
</tr>
<tr valign="top">
<td>35</td>
<td><em>HeldFirmOrderRequired</em></td>
<td><p>Can only be supplied against firm order: order line held awaiting customer response</p></td>
<td><p>104</p></td>
</tr>
<tr valign="top">
<td>36</td>
<td><em>HeldMinimumOrderReq</em></td>
<td><p>Minimum order value required: order line held</p></td>
<td><p>103</p></td>
</tr>
<tr valign="top">
<td>37</td>
<td><em>HeldPriceQuery</em></td>
<td><p>Price query: order line held awaiting customer response, usually because difference between order price and actual price exceeds agreed tolerance</p></td>
<td><p>903</p></td>
</tr>
<tr valign="top">
<td>38</td>
<td><em>HeldPriceIdentifierMismatch*</em></td>
<td><p>Price identifier invalid or doesn’t match price amount: order line held awaiting customer response.</p></td>
<td></td>
</tr>
<tr valign="top">
<td>39</td>
<td><em>HeldPromotionInvalid</em></td>
<td><p>Promotional deal invalid or expired: order line held awaiting customer response</p></td>
<td></td>
</tr>
<tr valign="top">
<td>40</td>
<td><em>HeldStockTaking*</em></td>
<td><p>Order line held – currently unavailable due to stock taking</p></td>
<td></td>
</tr>
<tr valign="top">
<td>41</td>
<td><em>NotFound</em></td>
<td><p>Order line not traced</p></td>
<td></td>
</tr>
<tr valign="top">
<td>42</td>
<td><em>NotOnBackorderFile</em></td>
<td><p>Response to order status enquiry or cancellation, for suppliers whose systems check enquiries or cancellations only against a backorder file. Where possible, the more informative responses NotFound and AlreadyShipped are preferred.</p></td>
<td></td>
</tr>
<tr valign="top">
<td>43</td>
<td><em>Processing</em></td>
<td><p>Ordered item(s) being processed by bookseller</p></td>
<td><p>410</p></td>
</tr>
<tr valign="top">
<td>44</td>
<td><em>ProcessingAwaitingBuyerInstruction</em></td>
<td><p>Ordered item(s) being processed by bookseller, awaiting buyer instruction</p></td>
<td><p>411</p></td>
</tr>
<tr valign="top">
<td>45</td>
<td><em>ReorderedSuppliedDamaged</em></td>
<td><p>Our supplier sent damaged item(s): reordered</p></td>
<td><p>407</p></td>
</tr>
<tr valign="top">
<td>46</td>
<td><em>ReorderedSuppliedImperfect</em></td>
<td><p>Our supplier sent imperfect item(s): reordered</p></td>
<td><p>408</p></td>
</tr>
<tr valign="top">
<td>47</td>
<td><em>ReorderedSuppliedShort</em></td>
<td><p>Our supplier sent short: reordered</p></td>
<td><p>406</p></td>
</tr>
<tr valign="top">
<td>48</td>
<td><em>ReorderedSupplierCannotTrace</em></td>
<td><p>Our supplier cannot trace order: reordered</p></td>
<td><p>409</p></td>
</tr>
<tr valign="top">
<td>49</td>
<td><em>ReorderedWrongItemSupplied</em></td>
<td><p>Our supplier sent wrong item(s): reordered</p></td>
<td><p>405</p></td>
</tr>
<tr valign="top">
<td>50</td>
<td><em>SubstBackordered</em></td>
<td><p>Substitute product backordered</p></td>
<td></td>
</tr>
<tr valign="top">
<td>51</td>
<td><em>SubstPartShippingPartBackordered</em></td>
<td><p>Substitute product part in process for immediate shipping, part backordered</p></td>
<td></td>
</tr>
<tr valign="top">
<td>52</td>
<td><em>SubstPartShippingPartCanceled</em></td>
<td><p>Substitute product part in process for immediate shipping, part cancelled</p></td>
<td></td>
</tr>
<tr valign="top">
<td>53</td>
<td><em>SubstShipping</em></td>
<td><p>Substitute product in process for immediate shipping</p></td>
<td></td>
</tr>
<tr valign="top">
<td>54</td>
<td><em>TemporaryHold</em></td>
<td><p>Order action not yet determined</p></td>
<td><p>999</p></td>
</tr>
<tr valign="top">
<td>55</td>
<td><em>HeldAdditionalServiceQuery</em></td>
<td><p>Order line held pending resolution of a query concerning an additional service requested for that order line</p></td>
<td></td>
</tr>
<tr valign="top">
<td>56</td>
<td><em>CanceledAdditionalServiceQuery</em></td>
<td><p>Order line cancelled due to a query concerning an additional service requested. The item should be re-ordered once the query has been resolved.</p></td>
<td></td>
</tr>
<tr valign="top">
<td>57</td>
<td><em>CanceledAccountStopped</em></td>
<td><p>Buyer’s account is temporarily stopped; the order line is cancelled, but product price and availability is reported.</p></td>
<td></td>
</tr>
<tr valign="top">
<td>58</td>
<td><em>AcceptedReadyForActivation*</em></td>
<td><p>Ordered digital item is ready for activation by the consumer</p></td>
<td></td>
</tr>
<tr valign="top">
<td>59</td>
<td><em>AcceptedActivated*</em></td>
<td><p>Ordered digital item is activated and ready for use by the consumer</p></td>
<td></td>
</tr>
<tr valign="top">
<td>60</td>
<td><em>AwaitingAuthorizationToShip*</em></td>
<td><p>Awaiting authorisation to despatch</p></td>
<td></td>
</tr>
</tbody>
</table>

**Table 2<a name="Table2"></a>: EDItEUR product availability codes**

This table defines a subset of ONIX codelist 65 used in
PublisherAvailabilityCode (page 19 line 18). The equivalent EDIFACT code
(List 8B) is shown for information only, as the ONIX code must always be
used.

| **ONIX** | **Description**                                                                                                                                                                                                                                                                                             | **Equivalent EDIFACT code (List 8B)** |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| 01       | Product was announced, and subsequently abandoned                                                                                                                                                                                                                                                           | AB                                    |
| 10       | Not yet available                                                                                                                                                                                                                                                                                           | NP                                    |
| 11       | Not yet available: will be stocked when available                                                                                                                                                                                                                                                           | NP                                    |
| 12       | Not yet available: will be published as print-on-demand only. May apply either to a POD successor to an existing conventional edition, when the successor will be published under a different ISBN (normally because different trade terms apply); or to a title that is being published as a POD original. | NP                                    |
| 20       | Available from us (form of availability unspecified)                                                                                                                                                                                                                                                        |                                       |
| 21       | Available from us from stock                                                                                                                                                                                                                                                                                | IB                                    |
| 22       | Available from us as a non-stock item, by special order                                                                                                                                                                                                                                                     | NQ / TO                               |
| 23       | Available from us by POD                                                                                                                                                                                                                                                                                    | MD                                    |
| 30       | Temporarily unavailable from us (reason unspecified)                                                                                                                                                                                                                                                        | TU                                    |
| 31       | Stock item, temporarily out of stock                                                                                                                                                                                                                                                                        | OB                                    |
| 32       | Temporarily unavailable, reprinting                                                                                                                                                                                                                                                                         | RP                                    |
| 33       | Temporarily unavailable, awaiting reissue                                                                                                                                                                                                                                                                   | RE                                    |
| 40       | Not available from us – reason unspecified                                                                                                                                                                                                                                                                  | NN / UC                               |
| 41       | This product is unavailable, but a successor product or edition is or will be available from us                                                                                                                                                                                                             | OR                                    |
| 42       | This product is unavailable, but the same content is or will be available from us in an alternative format                                                                                                                                                                                                  | OF                                    |
| 43       | This product has been transferred to another supplier                                                                                                                                                                                                                                                       | RF                                    |
| 44       | Not available to trade, apply direct to publisher                                                                                                                                                                                                                                                           | AD                                    |
| 45       | Must be bought as part of a set                                                                                                                                                                                                                                                                             | NS                                    |
| 46       | Withdrawn from sale, eg for legal reasons                                                                                                                                                                                                                                                                   | OP / RR                               |
| 47       | Remaindered                                                                                                                                                                                                                                                                                                 | RM                                    |
| 48       | Out of print, but a print-on-demand edition is or will be available under a different ISBN. Use only when the POD successor has a different ISBN, normally because different trade terms apply.                                                                                                             | OP / OF                               |
| 99       | Apply to customer service                                                                                                                                                                                                                                                                                   | CS                                    |

**Table 3<a name="Table3"></a>: Values for \<ProcessingInstructionCode\> element**

This table combines suggestions from BISAC committee members with
existing values from BIC TRADACOMS and EDItEUR EDIFACT formats. Wherever
it makes sense, each positive instruction code has a negative “Do
not...” counterpart, so that exceptions to an agreed processing
package can be specified. This is reflected in the list of values below.
The EDIFACT equivalent codes are shown purely for reference and must not
be used in
\<ProcessingInstructionCode\>.

| Description                                                                                                | *Code*                  |                                       |*EDI*<br/>*equiv*|*FACT*<br/>*alents*|
| ---------------------------------------------------------------------------------------------------------- | ----------------------- | ------------------------------------- | ---- | ----- |
|                                                                                                            | *Processing applied*    | *Processing cancelled / not applied*  |      |       |
| No processing / servicing                                                                                  |                         | *NoProcessing*                        |      | *NS*  |
| Supplier-applied copy number (for single copy only)                                                        | *AppliedCopyNumber*     | *NoAppliedCopyNumber*                 | *BB* | *BBN* |
| Start of supplier-applied copy number range                                                                | *AppliedCopyNumberFrom* |                                       |      |       |
| End of supplier-applied copy number range                                                                  | *AppliedCopyNumberTo*   |                                       |      |       |
| Apply security device, in accordance with separately agreed specification                                  | *SecurityDevice*        | *NoSecurityDevice*                    | *BS* | *BSN* |
| Plastic / Mylar jacket / sleeve, in accordance with separately agreed specification                        | *Jacket*                | *NoJacket*                            | *BJ* | *BJN* |
| Apply spine label, in accordance with separately agreed specification                                      | *SpineLabel*            | *NoSpineLabel*                        |      |       |
| Apply spine label, based upon string supplied in this copy detail                                          | *SpineLabelString*      |                                       |      |       |
| Apply pocket, in accordance with separately agreed specification                                           | *Pocket*                | *NoPocket*                            | *JK* | *JKN* |
| Apply circulation card, in accordance with separately agreed specification                                 | *CirculationCard*       | *NoCirculationCard*                   |      |       |
| Apply date due slip, in accordance with separately agreed specification                                    | *DateDueSlip*           | *NoDateDueSlip*                       |      |       |
| Apply binding/strengthening, in accordance with separately agreed specification                            | *Binding*               | *NoBinding*                           | *BI* | *BIN* |
| Stamp, in accordance with separately agreed specification                                                  | *Stamp*                 | *NoStamp*                             |      |       |
| Apply embossing, in accordance with separately agreed specification                                        | *Embossing*             | *NoEmbossing*                         |      |       |
| Apply RFID chip, in accordance with separately agreed specification                                        | *RFIDChip*              | *NoRFIDChip*                          |      |       |
| Provide audio/CD packaging, in accordance with separately agreed specification                             | *AudioPackaging*        | *NoAudioPackaging*                    | *BP* | *BPN* |
| Apply classification, in accordance with separately agreed specification                                   | *Classification*        | *NoClassification*                    | *BC* | *BCN* |
| Provide catalog record, in accordance with separately agreed specification                                 | *Catalog*               | *NoCatalog*                           | *CA* | *CAN* |
| Laminate (paperback) cover, in accordance with separately agreed specification                             | *Laminate*              | *NoLaminate*                          | *LA* | *LAN* |
| Apply sewn flexi binding, in accordance with separately agreed specification                               | *SewnFlexi*             | *NoSewnFlexi*                         | *SF* | *SFN* |
| Case bind paperback, in accordance with separately agreed specification                                    | *CaseBind*              | *NoCaseBind*                          | *TR* | *TRN* |
| Binding as supplied by publisher (cancels all processing related to binding, jacketing, reinforcement etc) |                         | *BindingAsSupplied*                   |      | *PF*  |
| Non-standard servicing – see instructions sent outside of EDI                                              | *SeparateInstructions*  |                                       | *NX* |       |

*Example of a Price fragment for a digital item in an Order Response XML
payload, for which a price identifier and associated lending terms are
specified:*

```
...
    <PriceConstraint>
      <PriceConstraintType>06</PriceConstraintType>
      <PriceConstraintStatus>02</PriceConstraintStatus>
      <PriceConstraintLimit>
        <Quantity>24</Quantity>
        <PriceConstraintUnit>10</PriceConstraintUnit>
      </PriceConstraintLimit>
      <PriceConstraintLimit>
        <Quantity>0</Quantity>
        <PriceConstraintUnit>07</PriceConstraintUnit>
      </PriceConstraintLimit>
      <PriceConstraintLimit>
        <Quantity>14</Quantity>
        <PriceConstraintUnit>09</PriceConstraintUnit>
      </PriceConstraintLimit>
    </PriceConstraint>
    <Price>
      <MonetaryAmount>25.99</MonetaryAmount>
      <PriceQualifierCode>06</PriceQualifierCode>
      <PriceIdentifier>
        <PriceIDType>05</PriceIDType>
        <IDTypeName>SupplierEBookPriceUID</IDTypeName>
        <IDValue>XXX-AAA</IDValue>
      </PriceIdentifier>
      <PriceTypeQualifier>10</PriceTypeQualifier>
    </Price>
...
```

<a name="Notes1"></a>1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

<a name="Notes2"></a>2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

<a name="Notes3"></a>3.  An ‘R’ in the right-most column means that the element is
    repeatable.

<a name="Notes4"></a>4.  It is recommended that HTTPS header-based authentication be used
    where possible.

<a name="Notes5"></a>5.  This element meets a theoretical requirement to allow a monetary
    value to be specified as either a per copy value or a total value
    for all copies in this copy detail. In practice, only percentages
    are known to be needed to meet current requirements.

<a name="Notes6"></a>6.  This element meets a theoretical requirement to allow a monetary
    value to be specified as either a per copy value or a total value
    for all copies in this copy detail. In practice, only percentages
    are known to be needed to meet current requirements.

<a name="Notes7"></a>7.  Code value ‘02’ in ResponsePurposeCode should only be returned if
    the Order Request contains the same number of order lines, with the
    same product identifier, quantity and references in each order line.
    Otherwise the correct response is to return ResponseCode with code
    value ‘10’ (duplicate order number – see Response header line 7).
