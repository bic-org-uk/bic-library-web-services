![BIC LOGO](assets/bic-logo.png)

### *Book Industry Communication*

### BIC Library Web Services API Standards

# Retrieve MARC Product Information

**Version 2.0, 29 October 2018**

**This document:** <http://www.bic.org.uk/files/pdfs/BICWSMARCProductInformation-V2.0.pdf>  
**XML schema:** <http://www.bic.org.uk/files/xml/BICWSMARCProductInformation-V2.0.xsd>  
**WSDL file:** <http://www.bic.org.uk/files/xml/BICWSMARCProductInformationSOAP-V2.0.wsdl>  
**XML namespace:** http://www.bic.org.uk/librarywebservices/marcProductInformation  
**Next review date:** 1 November 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve MARC Product
Information API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

A MARC Product Information Request may be implemented using either SOAP
or the basic HTTPS protocol\[1\] and POST method. The payload of a
Request may be formatted either as an XML document or as an equivalent
JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTPS and SOAP exchanges.

The complete specification of the MARC Product Information Request and
Response web service includes two machine-readable resources that are to
be used by implementers in conjunction with this document:

  - a WSDL Definition for the SOAP protocol versions of the web services

  - an XML Schema for Request and Response payloads in XML format.

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

**Business requirements **

The formats have been designed to support the implementation of web
services that accept product information requests and respond by
supplying product information in MARC format. Given the widespread use
of MARC in the UK for the supply of book product information to
libraries, a business case can be made for such a web service at a
number of points in the supply chain. Service providers are likely to
include wholesalers, distributors and others involved in library supply.

**Scope of the proposed formats**

The requirements for sending and responding to a MARC Product
Information Request can be defined relatively simply. There are only a
few options that a service provider would need to decide whether or not
to offer.

For the MARC Product Information Request application, the request format
is fully specified in this document, in separate versions for HTTP GET
and HTTP POST methods.

As well as including a header identifying the source and date-stamping
the response, the response format allows error conditions to be
reported, and provides an XML “wrapper” within which a MARC product
information record for each successful product request can be sent.
Implementations may determine the extent of the information they supply
about each product, but MARC product information records returned must
be valid according to either the MARC 21 or the UK MARC specification
and they should, if possible, meet BIC product data accreditation
standards.UK MARC is no longer supported and BIC recommends that MARC 21
be used where possible.

### REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**XML document encoding begins:** `<MARCProductInformationRequest version="2.0">...`

**JSON document encoding begins:** `{ "MARCProductInformationRequest": { "version": "2.0"...`

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
<td><p>A password to further authenticate the sender of the request<a href="#Notes4">[4]</a>.</p></td>
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
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier (see ONIX codelist 44). Mandatory if including an account identifier. Permitted values are:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li>
<li><em>09</em>&nbsp;&nbsp;ISIL</li>
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
<td><pre>RequestNumber</pre></td>
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
<td><p>Minimum acceptable MARC record quality, if not established by trading partner agreement. Permitted record qualities are:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Full MARC record</li>
<li><em>02</em>&nbsp;&nbsp;CIP record</li>
<li><em>03</em>&nbsp;&nbsp;Unspecified (lower than CIP accepted)</li></ul></td>
<td><p>D</p></td>
<td><pre>MARCRecordQuality</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Format in which the requestor would prefer product information records to be expressed in the response. Mandatory in every request.</p>
<ul><li><em>05</em>&nbsp;&nbsp;MARC 21 (only link included in response)</li>
<li><em>06</em>&nbsp;&nbsp;UKMARC (only link included in response)</li>
<li><em>07</em>&nbsp;&nbsp;MARCXML (MARC 21 in XML)</li>
<li><em>08</em>&nbsp;&nbsp;MARC 21, Base64-encoded</li>
<li><em>09</em>&nbsp;&nbsp;UKMARC, Base64-encoded</li>
<li><em>10</em>&nbsp;&nbsp;UNIMARC <em>(DEPRECATED)</em></li>
<li><em>11</em>&nbsp;&nbsp;UNIMARC (only link included in response)</li>
<li><em>12</em>&nbsp;&nbsp;UNIMARC, Base64-encoded</li></ul></td>
<td><p>M</p></td>
<td><pre>MARCRecordFormat</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Character encoding which the requester would prefer the MARC record to be encoded. If omitted, the requester has no preference. Permitted values:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary / other – description mandatory
<li><em>02</em>&nbsp;&nbsp;ISO 646 / ASCII</li>
<li><em>03</em>&nbsp;&nbsp;ISO 8859-2</li>
<li><em>04</em>&nbsp;&nbsp;Unicode / UTF-8</li>
<li><em>05</em>&nbsp;&nbsp;MARC-8</li>
<li><em>06</em>&nbsp;&nbsp;ISO 5426</li></ul></td>
<td><p>D</p></td>
<td><pre>MARCRecordCharEncoding</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>MARC record character encoding description. Mandatory if the requested character encoding is proprietary or not otherwise listed in line 8.</p></td>
<td><p>D</p></td>
<td><pre>MARCRecordCharEncodingDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Request detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Product</strong></p></td>
<td><p><strong>M</strong></p></td>
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
<td><p>Product ID type - see ONIX codelist 5</p></td>
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
<td><pre>Identifier</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Request XML payload using either the SOAP or the HTTPS
protocol:*

````
<MARCProductInformationRequest
  version="2.0"
  xmlns="http://www.bic.org.uk/librarywebservices/marcProductInformation">
  <Header>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
    <RequestNumber>001</RequestNumber>
    <IssueDateTime>20180418T1525</IssueDateTime>
    <MARCRecordQuality>02</MARCRecordQuality>
    <MARCRecordFormat>07</MARCRecordFormat>
  </Header>
  <Product>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9781234567890</IDValue>
    </ProductIdentifier>
  </Product>
</MARCProductInformationRequest>
````

*Example of a Request JSON payload using either the SOAP or the HTTP
protocol and the HTTP POST method:*

```
{"MARCProductInformationRequest": {
  "version": "2.0",
  "xmlns": "http://www.bic.org.uk/librarywebservices/marcProductInformation",
  "Header": {
    "AccountIdentifier": {
    "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "IssueDateTime": "20180418T1525",
    "MARCRecordQuality": "02"
    "MARCRecordFormat": "07"
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
Request uses the basic HTTPS protocol, the Response will be an XML or
JSON document as specified below attached to a normal HTTPS header. If
the Request uses the SOAP protocol, the Response will contain a SOAP
response message whose body will contain the XML or JSON document
specified below.


**XML document encoding begins:** `<MARCProductInformationResponse version="2.0">...`

**JSON document encoding begins:** `{ "MARCProductInformationResponse": { "version": "2.0"...`

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
<td><p><strong>M</strong></p></td>
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
<td><p>Sender ID type - see ONIX codelist 92</p></td>
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
<td><p>M</p></td>
<td><pre>IDValue</pre></td>
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
<td><p>Account identifier, required if included in the request</p></td>
<td><p>D</p></td>
<td><pre>AccountIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier (see ONIX codelist 44). Must be specified if an account identifier is specified. Permitted schemes are:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li>
<li><em>09</em>&nbsp;&nbsp;ISIL</li>
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
<td><p>References: request number of request must be quoted if included in the request; request date or date and time must be quoted in this composite if both number and date/time are included in the request.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>02</em>&nbsp;&nbsp;Number or date/time of associated product information request or search</li></ul></td>
<td><p>M</p></td>
<td><pre>ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference number / string</p></td>
<td><p>M</p></td>
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
<td><p>Reference date or date and time: must be quoted separately if and only if included in the request <em>without</em> a request number. See header line 5 for permitted formats.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Default currency of prices given in the response</p></td>
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
<td>Response type code. Suggested code values:<br />
<li><em>01</em>&nbsp;&nbsp;Service unavailable</li>
<li><em>02</em>&nbsp;&nbsp;Invalid ClientID or ClientPassword</li>
<li><em>03</em>&nbsp;&nbsp;Server unable to process request– a reason should normally be given as a free text description – see below</li>
<li><em>08</em>&nbsp;&nbsp;MARC record format or character encoding not as requested</li></ul></td>
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
<td><p>9</p></td>
<td><p>Format in which product information records are expressed in the response detail. Mandatory in every request that contains response detail elements.</p>
<ul><li><em>05</em>&nbsp;&nbsp;MARC 21 (only link included in response)</li>
<li><em>06</em>&nbsp;&nbsp;UK MARC (only link included in response)</li>
<li><em>07</em>&nbsp;&nbsp;MARCXML (MARC 21 in XML)</li>
<li><em>08</em>&nbsp;&nbsp;MARC 21, Base64-encoded</li>
<li><em>09</em>&nbsp;&nbsp;UK MARC, Base64-encoded</li>
<li><em>10</em>&nbsp;&nbsp;UNIMARC <em>(DEPRECATED)</em></li>
<li><em>11</em>&nbsp;&nbsp;UNIMARC (only link included in response)</li>
<li><em>12</em>&nbsp;&nbsp;UNIMARC, Base64-encoded</li></ul></td>
<td><p>D</p></td>
<td><pre>MARCRecordFormat</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Character encoding of the original MARC record prior to Base64 encoding</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary / other – description mandatory
<li><em>02</em>&nbsp;&nbsp;ISO 646 / ASCII</li>
<li><em>03</em>&nbsp;&nbsp;ISO 8859-2</li>
<li><em>04</em>&nbsp;&nbsp;Unicode / UTF-8</li>
<li><em>05</em>&nbsp;&nbsp;MARC-8</li>
<li><em>06</em>&nbsp;&nbsp;ISO 5426</li></ul></td>
<td><p>D</p></td>
<td><pre>MARCRecordCharEncoding</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Name of the character set of the original MARC record prior to Base64 encoding. Mandatory if the character encoding specified in line 11 is ‘01’.</p></td>
<td><p>D</p></td>
<td><pre>MARCRecordCharEncodingDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Response detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>MARC Product information record: mandatory unless the header reports an exception condition that prevents any response</strong></p></td>
<td><p><strong>D</strong></p></td>
<td><p><strong>MARCProductInformationRecord</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>EAN-13 product number as specified in the request detail, if any (mandatory unless trading partners have agreed to use an alternative product identifier). Mandatory if included in the request detail and a response code is included in this response detail.</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Alternative product identifier as specified in the request detail. Mandatory if included in the request detail and a response code is included in this response detail.</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Product ID type - see ONIX codelist 5</p></td>
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
<td><pre>Identifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Response code, if no information can be sent for this product. If present, no further elements may be included in this product information record.</p></td>
<td><p>D</p></td>
<td><pre>ResponseCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Response type code:</p>
<ul><li><em>06</em>&nbsp;&nbsp;Invalid product ID</li>
<li><em>07</em>&nbsp;&nbsp;No information for this product</li>
<li><em>08</em>&nbsp;&nbsp;Product record format not as requested</li></ul></td>
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
<td><p>MARC record encoding level (derived from MARC 21):</p>
<ul><li><em>#</em>&nbsp;&nbsp;Full level</li>
<li><em>1</em>&nbsp;&nbsp;Full level, material not examined</li>
<li><em>2</em>&nbsp;&nbsp;Less-than-full level, material not examined</li>
<li><em>3</em>&nbsp;&nbsp;Abbreviated level</li>
<li><em>4</em>&nbsp;&nbsp;Core level</li>
<li><em>5</em>&nbsp;&nbsp;Partial (preliminary) level</li>
<li><em>7</em>&nbsp;&nbsp;Minimal level</li>
<li><em>8</em>&nbsp;&nbsp;Pre-publication level</li></ul></td>
<td><p>D</p></td>
<td><pre>RecordEncodingLevel</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>URI for MARC record. May only be included for a record that is requested in un-encoded MARC 21 or UK MARC format.</p></td>
<td><p>D</p></td>
<td><pre><em>RecordURI</em></pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>The content of a record must comprise the content of a MARC product information record that conforms to the record format specified in the header. Each record must also be consistent with the specified record type. May only be included for a record that is requested in MARCXML format or Base64-encoded MARC 21 or UK MARC format.</p></td>
<td><p>D</p></td>
<td><pre><em>Record</em></pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Response XML payload:*

```
<MARCProductInformationResponse
  version="2.0"
  xmlns="http://www.bic.org.uk/librarywebservices/marcProductInformation">
  <Header>
    <IssueDateTime>20150418T1527</IssueDateTime>
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
      <ReferenceDateTime>20150418T152500</ReferenceDateTime>
    </ReferenceCoded>
    <RecordType>
      <RecordTypeCode>03</RecordTypeCode>
    </RecordType>
    <MARCRecordFormat>07</MARCRecordFormat>
  </Header>
  <MARCProductInformationRecord>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9781234567890</IDValue>
    </ProductIdentifier>
    <RecordEncodingLevel>1</RecordEncodingLevel>
    <Record>
      -- MARCXML RECORD HERE --
    </Record>
  </MARCProductInformationRecord>
</MARCProductInformationResponse>
```

*Example of a Response JSON payload:*

```
{"MARCProductInformationResponse": {
  "version": "2.0",
  "xmlns": "http://www.bic.org.uk/librarywebservices/marcProductInformation",
  "Header": {
    "IssueDateTime": "20150418T1527",
    "SenderIdentifier": {
      "SenderIDType": "01",
      "IDValue": "XYZ"
    },
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "ReferenceCoded": {
      "ReferenceCodeType": "01",
      "ReferenceNumber": "001",
      "ReferenceDateTime": "20150418T152500"
    },
    "MARCRecordFormat": "05"
  },
  "MARCProductInformationRecord": {
    "ProductIdentifier": {
      "ProductIDType": "03",
      "IDValue": "9781234567890"
    },
    "RecordEncodingLevel”: "1",
    "Record": "-- LINK TO MARC 21 RECORD HERE --"
  }
}}
```

<a name="Notes1"></a>1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

<a name="Notes2"></a>2.  In the column headed “M”, “M” means mandatory, and “D” means
    dependent.

<a name="Notes3"></a>3.  An ‘R’ in the right-most column means that the element is
    repeatable.

<a name="Notes4"></a>4.  It is recommended that HTTPS header-based authentication be used
    where possible.
