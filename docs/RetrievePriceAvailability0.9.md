![BIC LOGO](assets/bic-logo.png)

### *Book Industry Communication*

### BIC Library Web Services API Standards

# Retrieve Price and Availability

**Version 0.9, 8 March 2019**

**The published version of this document:** <http://www.bic.org.uk/files/pdfs/BICLWSPriceAvailability-V0.9.pdf>  
**XML schema:** <http://www.bic.org.uk/files/xml/BICLWSPriceAvailability-V0.9.xsd>  
**WSDL file:** <http://www.bic.org.uk/files/xml/BICLWSPriceAvailabilitySOAP-V0.9.wsdl>  
**XML namespace:** http://www.bic.org.uk/librarywebservices/priceandavailability  
**Next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve Price and Availability
API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

A Price and Availability (P\&A) Request may be implemented using either
SOAP or the basic HTTPS protocol[1](#Notes1) and POST method. The payload of a
P\&A Request may be formatted either as an XML document or as an
equivalent JSON document.

The same Response payload format options (payload in XML or JSON) will
apply to both basic HTTP and SOAP exchanges.

The complete specification of the Retrieve P\&A Request/Response web
service includes two machine-readable resources that are to be used by
implementers in conjunction with this document:

  - a WSDL Definition for the SOAP protocol version of the web service

  - an XML Schema for Requests and Response payloads in XML format.

It is strongly recommended that SOAP client implementations of this web
service be constructed using the BIC WSDL Definitions as a starting
point, as this will promote interoperability between SOAP client and
server implementations. In some development environments it may be
easier to implement a SOAP server without using the BIC WSDL
Definitions, but in this case care must be taken to ensure that the WSDL
Definitions that describe the actual implementation is functionally
equivalent to the BIC WSDL Definitions.

**Business requirements**

This API has been designed not only to allow libraries to find out
price, discount and availability from their suppliers, which will aid
decisions on where to purchase, but also to provide details about any
constraints and, for e-books, licenses that apply to the purchase. For
e-books available under different licenses this API is even more
important, as it provides the information required to allow the library
system to purchase the e-book under the correct license for their
requirements – budgetary or otherwise.

### REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**XML document encoding begins:** `<PriceAvailabilityRequest version="0.9">...`

**JSON document encoding begins:** `{ "PriceAvailabilityRequest": { "version": "0.9"...`

**Request document name and version**

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
<td><p>A unique identifier for the sender of the request. An alphanumeric string not containing spaces or punctuation</p></td>
<td><p>D</p></td>
<td><pre>ClientID</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>A password to further authenticate the sender of the request</p></td>
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
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li>
<li><em>11</em>&nbsp;&nbsp;PubEasy PIN</li></ul></td>
<td><p>M</p></td>
<td><pre>AccountIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Identification number / string of this request</p></td>
<td><p>D</p></td>
<td><pre>PriceAvailabilityRequestNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
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
<td><p>6</p></td>
<td><p>Supplier(s) for whom price and availability is requested, if not the web service host (use only for requests sent to aggregators).</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier ID type – see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Region(s) in which all supplier(s) to which the request applies must be located (use only for requests sent to aggregators)</p></td>
<td><p>D</p></td>
<td><pre>SupplierRegionsCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Region code scheme</p></td>
<td><p>M</p></td>
<td><pre>SupplierRegionCodeType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><ul><li><em>01</em>&nbsp;&nbsp;ISO 3166 country codes</li></ul></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Region codes, separated by commas</p></td>
<td><p>M</p></td>
<td><pre>RegionCodes</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Currency in which the requester would prefer prices to be quoted</p></td>
<td><p>D</p></td>
<td><pre>CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Request detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Product</strong></p></td>
<td><p>M</p></td>
<td><p><strong>Product</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>EAN-13 product number (mandatory unless trading partners have agreed to use an alternative product identifier)</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Alternative product identifier</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product ID type – see ONIX codelist 5</p></td>
<td><p>M</p></td>
<td><pre>ProductIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product number</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>If and only if this parameter is supported by the web service implementation, the quantity of single copies of the identified product to be supplied may be specified. An integer value must be specified.</p></td>
<td><p>D</p></td>
<td><pre>SupplyQuantity</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Request XML payload using either the SOAP or the HTTP
protocol and the HTTP POST method:*

```
<PriceAvailabilityRequest version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/priceandavailability">
  <Header>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
    <PriceAvailabilityRequestNumber>001</PriceAvailabilityRequestNumber>
    <IssueDateTime>20180418T152500</IssueDateTime>
    <SupplierIdentifier>
      <SupplierIDType>01</SupplierIDType>
      <IDValue>XYZ</IDValue>
    </SupplierIdentifier>
  </Header>
  <Product>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9781234567890</IDValue>
    </ProductIdentifier>
  </Product>
</PriceAvailabilityRequest>
```

*Example of a Request JSON payload using either the SOAP or the HTTP
protocol and the HTTP POST method:*

```
{"PriceAvailabilityRequest": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/priceandavailability",
  "Header": {
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "PriceAvailabilityRequestNumber": "001",
    "IssueDateTime": "20180418T152500",
    "SupplierIdentifier": {
      "SupplierIDType": "01",
      "IDValue": "XYZ"
    }
  },
  "Product": {
    "ProductIdentifier": {
      "ProductIDType": "03",
      "IDValue": "9781234567890"
    }
  }
}}
```

### RESPONSE

The Response will use the protocol corresponding to the Request. If the
Request uses the basic HTTP protocol, the Response will be an XML
document as specified below attached to a normal HTTP header. If the
Request uses the SOAP protocol, the Response will contain a SOAP
response message whose body will contain the XML document specified
below.

**XML document encoding begins:** `<PriceAvailabilityResponse version="0.9">...`

**JSON document encoding begins:** `{ "PriceAvailabilityResponse": { "version": "0.9"...`

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
<td><p><strong>Payload header</strong></p></td>
<td><p>M</p></td>
<td><p><strong>Header</strong></p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Document date/time: the date/time when the response was generated. Permitted formats are:</p>
<ul><li>YYYYMMDD</li>
<li>YYYYMMDDTHHMM</li>
<li>YYYYMMDDTHHMMZ (universal time)</li>
<li>YYYYMMDDTHHMM±HHMM (time zone)</li></ul>
<p>where "T" represents itself, ie letter T</p></td>
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
<td><p>Sender ID type – see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>SenderIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>D</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Identification number / string of this response</p></td>
<td><p>D</p></td>
<td><pre>PriceAvailabilityResponseNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Account identifier, required if included in the request</p></td>
<td><p>D</p></td>
<td><pre>AccountIdentifier</pre></td>
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
<td><pre>AccountIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>References: request number and/or date/time of request must be quoted if included in the request.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Request number or date/time of associated price and availability request</li></ul></td>
<td><p>M</p></td>
<td><pre>ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference number / string</p></td>
<td><p>D</p></td>
<td><pre>ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date or date and time</p></td>
<td><p>D</p></td>
<td><pre>ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Region(s) in which all supplier(s) to which the response applies must be located (for use by aggregators when responses include information from multiple suppliers located in different regions).</p></td>
<td><p>D</p></td>
<td><pre>SupplierRegionsCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Region code scheme</p></td>
<td><p>M</p></td>
<td><pre>SupplierRegionCodeType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><ul><li><em>01</em>&nbsp;&nbsp;ISO 3166 country codes</li></ul></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Region codes, separated by spaces</p></td>
<td><p>M</p></td>
<td><pre>RegionCodes</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Default currency of prices given in the response – mandatory if response code ‘05’ is included.</p></td>
<td><p>D</p></td>
<td><pre>CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Response code, if there are exception conditions that affect the response as a whole</p></td>
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
<li><em>04</em>&nbsp;&nbsp;No information for supplier(s) listed below</li>
<li><em>05</em>&nbsp;&nbsp;Prices not quoted in preferred currency</li></ul></td>
<td><p>M</p></td>
<td><pre>ResponseType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Free text description / reason for response</p></td>
<td><p>D</p></td>
<td><pre>ResponseTypeDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier identifier, if the exception condition affects a specified supplier</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier ID type – see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Report detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Product price and availability: mandatory unless the header reports an exception condition that prevents any response</strong></p></td>
<td><p>D</p></td>
<td><p><strong>ProductPriceAvailability</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>EAN-13 product number (mandatory unless trading partners have agreed to use an alternative product identifier)</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Alternative product identifier</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product ID type – see ONIX codelist 5</p></td>
<td><p>M</p></td>
<td><pre>ProductIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product number</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Response code, if either no information can be sent for this product or the quoted currency is not the requester’s preferred currency. If code value is ‘06’ or ‘07’, elements 4 to 8 below must not be sent.</p></td>
<td><p>D</p></td>
<td><pre>ResponseCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Response type code. Suggested code values:</p>
<ul><li><em>05</em>&nbsp;&nbsp;Price not quoted in preferred currency</li>
<li><em>06</em>&nbsp;&nbsp;Invalid product ID</li>
<li><em>07</em>&nbsp;&nbsp;No information for this product</p></li></ul></td>
<td><p>M</p></td>
<td><pre>ResponseType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Free text description</p></td>
<td><p>D</p></td>
<td><pre>ResponseTypeDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Height in mm</p></td>
<td><p>D</p></td>
<td><pre>Height</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Width in mm</p></td>
<td><p>D</p></td>
<td><pre>Width</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Depth in mm</p></td>
<td><p>D</p></td>
<td><pre>Depth</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Unit weight in gm</p></td>
<td><p>D</p></td>
<td><pre>UnitWeight</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Supplier price and availability – <em>for details see below.</em></p></td>
<td><p>D</p></td>
<td><pre>SupplierPriceAvailability</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Supplier price and availability</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Supplier price and availability – repeatable for each supplier if the response covers more than one source for the requested item</strong></p></td>
<td><p><strong>D</strong></p></td>
<td><p><strong>ProductPriceAvailability.SupplierPriceAvailability</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Date and possibly time when the price and availability information contained in this supplier’s response was last updated</p>
(YYYYMMDD or YYYYMMDDTHHMMSS)</td>
<td><p>D</p></td>
<td><pre>LastUpdated</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Supplier identifier, if web service host is an aggregator responding on behalf of a supplier</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier ID type – see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Location(s) from which supplier is able to ship the item.</p></td>
<td><p>D</p></td>
<td><pre>SupplierLocation</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location identifier</p></td>
<td><p>D</p></td>
<td><pre>LocationIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location ID type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</p></li></ul></td>
<td><p>M</p></td>
<td><pre>LocationIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary ID type name</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location ID value</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location name</p></td>
<td><p>D</p></td>
<td><pre>LocationName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Quantity requested, if applicable – see Note</p></td>
<td><p>D</p></td>
<td><pre>SupplyQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>In stock indicator. Suggested code values:</p>
<ul><li><em>01</em>&nbsp;&nbsp;In stock, quantity unspecified</li>
<li><em>02</em>&nbsp;&nbsp;Out of stock</li>
<li><em>03</em>&nbsp;&nbsp;Requested quantity available</li>
<li><em>04</em>&nbsp;&nbsp;Requested quantity unavailable</li></ul></td>
<td><p>D</p></td>
<td><pre>InStock</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Product availability status</p></td>
<td><p>D</p></td>
<td><p>AvailabilityCoded</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier item availability code value. See <a href="#Table1">Table 1</a> for BIC web services availability status codes that should be used in this context.</p></td>
<td><p>M</p></td>
<td><p>SupplierAvailabilityCode</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publisher/distributor product availability code value – see ONIX codelist 65</p></td>
<td><p>D</p></td>
<td><p>ProductAvailabilityCode</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Availability date</p></td>
<td><p>D</p></td>
<td><pre>ExpectedShipDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publishing status code value – see ONIX codelist 64</p></td>
<td><p>D</p></td>
<td><p>PublishingStatusCode</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>On-display date for the product, if ordered (YYYYMMDD) – included if display is embargoed until the specified date, or if mandated by a publishing status code value.</p></td>
<td><p>D</p></td>
<td><p>LibraryOnDisplayDate</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Expected order time for a non-stock item – in days</p></td>
<td><p>D</p></td>
<td><p>OrderTime</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Successor product</p></td>
<td><p>D</p></td>
<td><pre>SuccessorProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product ID type – see ONIX codelist 5</p></td>
<td><p>M</p></td>
<td><pre>ProductIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product number</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Successor product form – see ONIX codelist 150</p></td>
<td><p>D</p></td>
<td><pre>SuccessorProductForm</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Alternative format product</p></td>
<td><p>D</p></td>
<td><pre>AlternativeProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product ID type – see ONIX codelist 5</p></td>
<td><p>M</p></td>
<td><pre>ProductIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product number</p></td>
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Alternative product form – see ONIX codelist 150</p></td>
<td><p>D</p></td>
<td><pre>AlternativeProductForm</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>For digital items, the technical protection method(s) applied as a condition of sale at the supplier’s price identifier. Repeatable – use ONIX code list 144.</p></td>
<td><p>D</p></td>
<td><p>EpubTechnicalProtection</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Usage constraint(s) associated with the supplier’s identified price. Repeatable</p></td>
<td><p>D</p></td>
<td><p>PriceConstraint</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of constraint – use ONIX code list 230.</p></td>
<td><p>M</p></td>
<td><p>PriceConstraintType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Status of constraint – use ONIX code list 146.</p></td>
<td><p>M</p></td>
<td><p>PriceConstraintStatus</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Limit of constraint</p></td>
<td><p>D</p></td>
<td><p>PriceConstraintLimit</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Limiting quantity</p></td>
<td><p>M</p></td>
<td><p>Quantity</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity unit – use ONIX code list 147.</p></td>
<td><p>M</p></td>
<td><p>PriceConstraintUnit</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>For digital items, the licensing terms applicable as a condition of sale at the supplier’s identified price.</p></td>
<td><p>D</p></td>
<td><p>EpubLicense</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>License name</p></td>
<td><p>M</p></td>
<td><p>EpubLicenseName</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>License expression</p></td>
<td><p>D</p></td>
<td><p>EpubLicenseExpression</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Expression type – use ONIX code list 218.</p></td>
<td><p>M</p></td>
<td><p>EpubLicenseExpressionType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Expression type name</p></td>
<td><p>D</p></td>
<td><p>EpubLicenseExpressionTypeName</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>URI of license expression</p></td>
<td><p>M</p></td>
<td><p>EpubLicenseExpressionLink</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Further conditions applicable to the supplier’s identified price.</p></td>
<td><p>D</p></td>
<td><p>PriceCondition</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of condition – use ONIX code list 167.</p></td>
<td><p>M</p></td>
<td><p>PriceConditionType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity associated with condition</p></td>
<td><p>D</p></td>
<td><p>PriceConditionQuantity</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of quantity – use ONIX code list 168.</p></td>
<td><p>M</p></td>
<td><p>PriceConditionQuantityType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity</p></td>
<td><p>M</p></td>
<td><p>Quantity</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity unit – use ONIX code list 169.</p></td>
<td><p>M</p></td>
<td><p>QuantityUnit</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>15</p></td>
<td><p>Expected unit price. Price may be repeated if more than one price type or currency is to be included.</p></td>
<td><p>D</p></td>
<td><p>Price</p></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier’s price identifier. Must be included if the product is available at various identified prices according to the terms, conditions and constraints of supply. If both price amount and price identifier are specified, the buyer must ensure they are consistent. The price identifier must match a price identifier specified in the current ONIX record for the same item.</p></td>
<td><p>D</p></td>
<td><p>PriceIdentifier</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary price identifier scheme – use ONIX code list 217</p></td>
<td><p>M</p></td>
<td><p>PriceIDType</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Proprietary scheme name</p></td>
<td><p>D</p></td>
<td><p>IDTypeName</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Identifier value</p></td>
<td><p>M</p></td>
<td><p>IDValue</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price amount</p></td>
<td><p>D</p></td>
<td><p>MonetaryAmount</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Currency: ISO 4217 currency code</p></td>
<td><p>D</p></td>
<td><p>CurrencyCode</p></td>
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
<td><p>PriceQualifierCode</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price qualifier corresponding to the supplier’s price identifier – use ONIX code list 59.</p></td>
<td><p>D</p></td>
<td><p>PriceTypeQualifier</p></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Price discount percent (specific to the requester)</p></td>
<td><p>D</p></td>
<td><pre>DiscountPercentage</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Response XML payload:*

```
<PriceAvailabilityResponse version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/priceandavailability">
  <Header>
    <IssueDateTime>20180424T1145</IssueDateTime>
    <SenderIdentifier>
      <SenderIDType>01</SenderIDType>
      <IDValue>XYZ</IDValue>
    </SenderIdentifier>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
    <ReferenceCoded>
      <ReferenceCodeType>01</ReferenceCodeType>
      <ReferenceNumber>001</ReferenceNumber>
      <ReferenceDateTime>20180418T152500</ReferenceDateTime>
    </ReferenceCoded>
  </Header>
  <ProductPriceAvailability>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780123456789</IDValue>
    </ProductIdentifier>
    <SupplierPriceAvailability>
      <SupplierIdentifier>
        <SupplierIDType>01</SupplierIDType>
        <IDValue>XYZ</IDValue>
      </SupplierIdentifier>
      <AvailabilityCoded>
        <SupplierAvailabilityCode>20</SupplierAvailabilityCode>
        <PublisherAvailabilityCode>21</PublisherAvailabilityCode>
      </AvailabilityCoded>
      <Price>
        <MonetaryAmount>19.99</MonetaryAmount>
        <DiscountPercentage>15</DiscountPercentage>
      </Price>
    </SupplierPriceAvailability>
  </ProductPriceAvailability>
</PriceAvailabilityResponse>
```

*Example of a Response JSON payload:*

```
{"PriceAvailabilityResponse": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/priceandavailability",
  "Header": {
    "IssueDateTime": "20180424T1145",
    "SenderIdentifier": {
      "SenderIDType": "01",
      "IDValue": "XYZ"
    },
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": 12345
    },
    "ReferenceCoded": {
      "ReferenceCodeType": "01",
      "ReferenceNumber": "001",
      "ReferenceDateTime": "20180418T152500"
    }
  },
  "ProductPriceAvailability": {
    "ProductIdentifier": {
      "ProductIDType": "03",
      "IDValue": "9780123456789"
    },
    "SupplierPriceAvailability": {
      "SupplierIdentifier": {
        "SupplierIDType": "01",
        "IDValue": "XYZ"
      },
      "AvailabilityCoded": {
        "SupplierAvailabilityCode": "20",
        "PublisherAvailabilityCode": "21"
      },
      "Price": {
        "MonetaryAmount": 19.99,
        "DiscountPercentage": 15
      }
    }
  }
}}
```

**Table 1<a name="Table1"></a>: Supplier item availability codes**

Used in SupplierAvailabilityCode. This code list is
derived from ONIX codelist 65, but with significant differences for code
values 31 and
above.

| **Code value** | **Description**                                                                                                                                                                  |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 10             | Not yet available - reason may be provided by publisher product availability code (e.g. 10, 11 or 12) and/or publishing status code (e.g. 02)                                    |
| 20             | Available - further information on the precise nature of the availability should normally be provided by publisher/distributor product availability code (e.g. 20, 21, 22 or 23) |
| 21             | Available - from stock - no additional availability information would normally be provided                                                                                       |
| 23             | Available - manufactured on demand                                                                                                                                               |
| 30             | Temporarily unavailable - reason may be provided by publisher product availability code                                                                                          |
| 31             | Temporarily unavailable due to stock taking                                                                                                                                      |
| 40             | Not available - if due to a supply chain issue, the reason should be provided by publisher product availability code and/or publishing status code                               |
| 41             | Not available - publisher address unknown                                                                                                                                        |
| 42             | Not available - publisher no longer trading                                                                                                                                      |
| 43             | Not available - rights restricted                                                                                                                                                |
| 44             | Not available in pack/set form - only available singly                                                                                                                           |
| 80             | Sold - second-hand or antiquarian item                                                                                                                                           |
| 90             | Availability uncertain - no further information                                                                                                                                  |
| 91             | Availability uncertain - item not known / identifier not recognised                                                                                                              |
| 92             | Availability uncertain - apply to customer services                                                                                                                              |

#### Notes

<a name="Notes1"></a>1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

<a name="Notes2"></a>2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

<a name="Notes3"></a>3.  An ‘R’ in the right-most column means that the element is
    repeatable.
