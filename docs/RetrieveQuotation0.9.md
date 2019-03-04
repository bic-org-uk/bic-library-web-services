![BIC LOGO](assets/bic-logo.png)

### *Book Industry Communication*

### BIC Library Web Services API Standards

# Retrieve Quotation

**Version 0.9, 28 September 2018**

**This document:** http://www.bic.org.uk/files/pdfs/BICLWSRetrieveQuote-V0.9.pdf  
**XML schema:** http://www.bic.org.uk/files/xml/BICLWSRetrieveQuote-V0.9.xsd  
**WSDL file:** http://www.bic.org.uk/files/xml/BICLWSRetrieveQuoteSOAP-V0.9.wsdl  
**XML namespace:** http://www.bic.org.uk/librarywebservices/quotation  
**Latest next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve Quotation API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

A Retrieve Quotation Request may be implemented using either SOAP or the
basic HTTPS protocol\[1\] and POST method. The payload of a Retrieve
Quotation Request may be formatted either as an XML document or as an
equivalent JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTPS and SOAP exchanges.

The complete specification of the Retrieve Quotation Request/Response
web service includes two machine-readable resources that are to be used
by implementers in conjunction with this document:

  - a WSDL Definition for the SOAP protocol version of the web service

  - an XML Schema for Requests and Response payloads in XML format.

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

### Business requirements

There are several use cases in which a supplier prepares a quotation for
a library buyer. Some of these can be initiated by the supplier and some
are in response to action by the library:

  - Showroom Visit

  - Supplier Selection

  - Approvals

  - Standing Orders

  - Supplier’s Website orders

In all these cases the library needs to be able to retrieve a quotation
from the supplier in order to use the information that it contains to
prepare orders. To retrieve a quotation the library first requests a
list of quotes prepared by the supplier, then requests the quotation
from that list. As this is a realtime service, quotations are not
batched for delivery by the supplier, but are supplied one at a time on
request.

### REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**XML document encoding begins:** `<QuotationRequest version="0.9">...`

**JSON document encoding begins:** `{ "QuotationRequest": { "version": "0.9"...`

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
<td>1</td>
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
<td><p>Account identifier.</p></td>
<td><p>D</p></td>
<td><pre>AccountIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>A code value from a BIC-controlled codelist for the scheme used for the account identifier (see ONIX codelist 44). Permitted schemes are:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Proprietary</li>
<li><em>06</em>&nbsp;&nbsp;EAN-UCC GLN</li>
<li><em>07</em>&nbsp;&nbsp;SAN</li>
<li><em>11</em>&nbsp;&nbsp;PubEasy PIN</li></ul></td>
<td><p>M</p></td>
<td><pre>  AccountIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
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
<td>5</td>
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
<td><p>References.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>35</em>&nbsp;&nbsp;Library’s supplier reference</li>
<li><em>36</em>&nbsp;&nbsp;Supplier’s library customer reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Reference</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Reference date-time (for format options see line 5)</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><p>Supplier to whom this request should be forwarded, if it is not addressed to the web service host (use only for requests sent to aggregation services).</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Supplier ID type - see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>  SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Quotation number</p></td>
<td><p>M</p></td>
<td><pre>QuotationReference</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Retrieve Quotation Request XML payload using either the
SOAP or the HTTPS protocol and the POST method:*

```
<QuotationRequest version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/quotation">
  <AccountIdentifier>
    <AccountIDType>01</AccountIDType>
    <IDValue>12345</IDValue>
  </AccountIdentifier>
  <RequestNumber>001</RequestNumber>
  <IssueDateTime>20180422T1525</IssueDateTime>
  <QuotationReference>Q12345</QuotationReference>
</QuotationRequest>
```

*JSON equivalent of the above payload:*

```
{
  "QuotationRequest": {
    "version": "0.9",
    "xmlns": "http://www.bic.org.uk/librarywebservices/quotation",
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "IssueDateTime": "20180422T1525",
    "QuotationReference": "Q12345"
  }
}
```

### RESPONSE

The Response will use the protocol corresponding to the Request. If the
Request uses the basic HTTPS protocol, the Response will be an XML or
JSON document as specified below attached to a normal HTTPS header. If
the Request uses the SOAP protocol, the Response will contain a SOAP
response message whose body will contain the XML or JSON document
specified below.

**XML document encoding begins:** `<QuotationResponse version="0.9">...`

**JSON document encoding begins:** `{ "QuotationResponse": { "version": "0.9"...`

**Header**

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
<td><p><strong>M</strong></p></td>
<td><p><strong>Header</strong></p></td>
<td></td>
</tr>
<tr valign="top">
<td>1</td>
<td><p>Document date/time: the date/time when the quotation was generated. Permitted formats are:</p>
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
<td><p></p></td>
<td><p>Sender ID type - see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>  SenderIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>ID type name, only if ID type = proprietary</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p>Account identifier.</p></td>
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
<li><em>11</em>&nbsp;&nbsp;PubEasy PIN</li></ul></td>
<td><p>M</p></td>
<td><pre>  AccountIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Quotation number. Must match the quotation reference in the request.</p></td>
<td><p>M</p></td>
<td><pre>QuotationNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>References: request number and/or date/time of request must be quoted if included in the request.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Number or date/time of associated quotation request</li>
<li><em>11</em>&nbsp;&nbsp;Buyer’s order reference</li>
<li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>17</em>&nbsp;&nbsp;Promotion or deal reference</li>
<li><em>23</em>&nbsp;&nbsp;Supplier’s order reference</li>
<li><em>32</em>&nbsp;&nbsp;Buyer’s authorisation for expense</li>
<li><em>35</em>&nbsp;&nbsp;Library’s supplier reference</li>
<li><em>36</em>&nbsp;&nbsp;Supplier’s library customer reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Reference number / string</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Reference date or date and time. Mandatory if an IssueDateTime is included in the request. See Header line 1 for format options.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>7</td>
<td><p>Response function – normally only sent if the same order request is repeated.</p>
<ul><li><em>01</em>  Response sent for the first time (default)</li>
<li><em>02</em>  Duplicate response to a repeated quotation request</li></ul></td>
<td><p>D</p></td>
<td><pre>ResponsePurposeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>Response code, if there are exception conditions.</p></td>
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
<li><em>11</em>&nbsp;&nbsp;Invalid quotation number</li>
<li><em>16</em>&nbsp;&nbsp;Invalid or unknown account or supplier identifier</li>
<li><em>19</em>&nbsp;&nbsp;Server unable to process request – unable to contact supplier</td></li></ul>
<td><p>M</p></td>
<td><pre>  ResponseType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Free text description / reason for response</p></td>
<td><p>D</p></td>
<td><pre>  ResponseTypeDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Supplier identifier (only included if specified in the request; mandatory if the response type code is ‘19’)</p></td>
<td><p>D</p></td>
<td><pre>SupplierIdentifier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Supplier ID type - see ONIX codelist 92</p></td>
<td><p>M</p></td>
<td><pre>  SupplierIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>ID type name, only if Supplier ID type is proprietary</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Identifier</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><pre>10</pre></td>
<td><p>Quotation type. Mandatory unless there are exception conditions.</p>
<ul><li><em>01</em>&nbsp;&nbsp;Customer-selected firm order</li>
<li><em>02</em>&nbsp;&nbsp;Customer-selected proposed order</li>
<li><em>03</em>&nbsp;&nbsp;Response to customer request for quotation</li>
<li><em>04</em>&nbsp;&nbsp;Supplier-selected firm order</li>
<li><em>05</em>&nbsp;&nbsp;Supplier-selected proposed order</li></ul></td>
<td><p>D</p></td>
<td><pre>QuotationType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><pre>12</pre></td>
<td><p>Quotation currency</p>
<p>Values: ISO 4217 currency codes</p></td>
<td><p>D</p></td>
<td><pre>CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Order qualifying dates</p></td>
<td><p>D</p></td>
<td><pre>DateCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Date YYYYMMDD</p></td>
<td><p>M</p></td>
<td><pre>  Date</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Order qualifying date type:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Cancel if not shipped by</li>
<li><em>03</em>&nbsp;&nbsp;Fill all available by, cancel remainder</li></ul></td>
<td><p>M</p></td>
<td><pre>  DateQualifierCode</pre></td>
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
<td><p></p></td>
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
<td><p></p></td>
<td><p>Identifier string</p></td>
<td><p>M</p></td>
<td><pre>    IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Party name</p></td>
<td><p>D</p></td>
<td><pre>  PartyName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Postal address</p></td>
<td><p>D</p></td>
<td><pre>  PostalAddress</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Address line</p></td>
<td><p>M</p></td>
<td><pre>    AddressLine</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>Communication locator string</p></td>
<td><p>M</p></td>
<td><pre>    CommunicationLocator</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Contact details</p></td>
<td><p>D</p></td>
<td><pre>  ContactPerson</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<ul><li><em>01</em>&nbsp;&nbsp;Next day (other codes to be added)</li></ul></td>
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
<td><p></p></td>
<td><p>Use specified carrier</p></td>
<td><p>D</p></td>
<td><pre>  Carrier</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>Carrier name code</p></td>
<td><p>M</p></td>
<td><pre>      CarrierNameCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Name of carrier</p></td>
<td><p>D</p></td>
<td><pre>      CarrierName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Carrier’s delivery service code / name</p></td>
<td><p>D</p></td>
<td><pre>    CarrierService</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Free text delivery instruction</p></td>
<td><p>D</p></td>
<td><pre>  DeliveryNotes</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>17</p></td>
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
<td><pre>19</pre></td>
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
<td><p></p></td>
<td><p>Number of days from date of invoice, or</p></td>
<td><p>D</p></td>
<td><pre>  NetDaysDue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Due date YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>  NetDueDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><pre>21</pre></td>
<td>% discount expected to apply to this order – decimal number between 0 and 100</td>
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
<td colspan="5"><h4>Item detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Quotation detail</strong></p></td>
<td><p><strong>M</strong></p></td>
<td><p><strong>ItemDetail</strong></p></td>
<td><p><strong>R</strong><p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Quotation line number</p></td>
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
<td><p>Alternative product identifier</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Product ID type – use ONIX Code List 5</p></td>
<td><p>M</p></td>
<td><pre>  ProductIDType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>ID type name, only if ID type = ‘01’ (proprietary)</p></td>
<td><p>D</p></td>
<td><pre>  IDTypeName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>“Bib number” – unique catalogue record number assigned by the library</p></td>
<td><p>D</p></td>
<td><pre>  BibNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Product form code – use ONIX code list 150</p></td>
<td><p>D</p></td>
<td><pre>  ProductForm</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Title: text</p></td>
<td><p>D</p></td>
<td><pre>  Title</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Author name (repeatable): text</p></td>
<td><p>D</p></td>
<td><pre>  Author</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Series title: text</p></td>
<td><p>D</p></td>
<td><pre>  SeriesTitle</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Volume or part designation: text</p></td>
<td><p>D</p></td>
<td><pre>  VolumeOrPart</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Edition statement: text</p></td>
<td><p>D</p></td>
<td><pre>  EditionStatement</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Place of publication: text</p></td>
<td><p>D</p></td>
<td><pre>  CityOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Country of publication: text</p></td>
<td><p>D</p></td>
<td><pre>  CountryOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Publisher: text</p></td>
<td><p>D</p></td>
<td><pre>  PublisherName</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Date of publication: YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>  DateOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Year of publication: YYYY</p></td>
<td><p>D</p></td>
<td><pre>  YearOfPublication</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>Quantity quoted</p></td>
<td><p>M</p></td>
<td><pre>QuotationQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Quotation line references If included, must contain a reference number or a reference date or both.</p></td>
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
<li><em>31</em>&nbsp;&nbsp;Catalogue or price list reference</li>
<li><em>32</em>&nbsp;&nbsp;Authorisation for expense reference</li>
<li><em>33</em>&nbsp;&nbsp;Buyer’s internal supplier reference</li>
<li><em>34</em>&nbsp;&nbsp;Library new title list reference</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Reference number / string</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date-time (for format options see Header line 6)</p></td>
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
<td><p></p></td>
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
<td><p>10</p></td>
<td><p>For digital items, the technical protection method(s) applied as a condition of sale at the supplier’s price identifier. Repeatable – use ONIX code list 144.</p></td>
<td><p>D</p></td>
<td><pre>EpubTechnicalProtection</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Usage constraint(s) associated with the supplier’s identified price. Repeatable</p></td>
<td><p>D</p></td>
<td><pre>PriceConstraint</pre></td>
<td><p>R</p></td>
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
<td><p>R</p></td>
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
<td><p>12</p></td>
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
<td><p>R</p></td>
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
<td><p>13</p></td>
<td><p>Further conditions applicable to the supplier’s identified price.</p></td>
<td><p>D</p></td>
<td><pre>PriceCondition</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of condition – use ONIX code list 167.</p></td>
<td><p>M</p></td>
<td><pre>  PriceConditionType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity associated with condition</p></td>
<td><p>D</p></td>
<td><pre>  PriceConditionQuantity</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of quantity – use ONIX code list 168.</p></td>
<td><p>M</p></td>
<td><pre>    PriceConditionQuantityType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity</p></td>
<td><p>M</p></td>
<td><pre>    Quantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Quantity unit – use ONIX code list 169.</p></td>
<td><p>M</p></td>
<td><pre>    QuantityUnit</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><p>Expected unit price. Price may be repeated if more than one price type or currency is to be included.</p></td>
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
<td><p></p></td>
<td><p>Price amount</p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p>15</p></td>
<td><p>Product availability status, if needed.</p></td>
<td><p>D</p></td>
<td><pre>AvailabilityCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p><a name="SupplierAvailabilityCode"></a>Supplier item availability code value. See <a href="#Table1">Table 1</a> for BIC web services availability status codes. Only a limited range of these codes would be expected to be used in a quotation.</p></td>
<td><p>M</p></td>
<td><pre>  SupplierAvailabilityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publisher/distributor product availability code value – see ONIX codelist 65. Only a limited range of these codes would be expected to be used in a quotation.</p></td>
<td><p>D</p></td>
<td><pre>  PublisherAvailabilityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Availability date</p></td>
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
<td><p>On-display date for the product, if ordered (YYYYMMDD) – included if display is embargoed until the specified date, or if mandated by a publishing status code value.</p></td>
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
<td><pre>16</pre></td>
<td><p>Invoicing instructions</p>
<ul><li><em>04</em>&nbsp;&nbsp;Invoice processing charges separately</li>
<li><em>05</em>&nbsp;&nbsp;Invoice this order line separately</li></ul></td>
<td><p>D</p></td>
<td><pre>InvoicingInstructionsCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>17</p></td>
<td><p>All-copy detail – see below.</p></td>
<td><p>D</p></td>
<td><pre>AllCopyDetail</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Copy detail – see below.</p></td>
<td><p>D</p></td>
<td><pre>CopyDetail</pre></td>
<td><p>R</p></td>
</tr>
</tbody>
</table>

### All-copy detail

The all-copy detail section may only occur once within `<ItemDetail>`,
when the entire quantity of ordered copies of an item share the same
details. In cases where some details do not apply to all copies, they
should be specified in instances of the copy detail section as specified
below.

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
<td></td>
<td><p>All-copy detail</p></td>
<td><p>D</p></td>
<td><p>ItemDetail.AllCopyDetail</p></td>
<td></td>
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
<td><p></p></td>
<td><p>Collection code: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>  CollectionCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>Classification scheme version number</p></td>
<td><p>D</p></td>
<td><pre>  SubjectSchemeVersion</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>Value amount</p></td>
<td><p>M</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><pre>17</pre></td>
<td><p>Special processing / servicing instruction (repeatable). For values, see <a href="#Table2">Table 2</a></p></td>
<td><p>D</p></td>
<td><pre>ProcessingInstructionCode</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Supplier-applied copy number. Mandatory immediately following any instance of ProcessingInstructionCode whose code value is any of AppliedCopyNumber, AppliedCopyNumberFrom or AppliedCopyNumberTo.</p></td>
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
<td><p></p></td>
<td><p>Buyer’s fund/budget number: a string defined by the library</p></td>
<td><p>M</p></td>
<td><pre>  FundNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Buyer’s fund description: text</p></td>
<td><p>D</p></td>
<td><pre>  FundDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Percentage to charge to this fund: a decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>  Percent</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Amount to charge to this fund, in order currency: a decimal number[4]</p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p>23</p></td>
<td><p>Copy/ies specified in the copy detail section requested by (repeatable): text</p></td>
<td><p>D</p></td>
<td><pre>RequestedBy</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td><p>24</p></td>
<td><p>Copy/ies specified in the copy detail section approved by: text</p></td>
<td><p>D</p></td>
<td><pre>ApprovedBy</pre></td>
<td></td>
</tr>
</tbody>
</table>

### Copy detail

The copy detail section is a repeatable element within <ItemDetail>
and refers to a part order line: those copies of an order line item that
share the same details. If all copies if an order line share the same
details, these should be contained in an all-copy detail section as
specified above. The sum of all part order line quantities as specified
in each <CopyQuantity> must equal the total order line quantity as
specified in <OrderQuantity>.

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
<td></td>
<td><p>Copy detail</p></td>
<td><p>D</p></td>
<td><p>ItemDetail.CopyDetail</p></td>
<td><p>R</p></td>
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
<td><p></p></td>
<td><p>Collection code: a string defined by the library</p></td>
<td><p>D</p></td>
<td><pre>  CollectionCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>Classification scheme version number</p></td>
<td><p>D</p></td>
<td><pre>  SubjectSchemeVersion</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p></p></td>
<td><p>Value amount</p></td>
<td><p>M</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
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
<td><p>Special processing / servicing instruction (repeatable). For values, see <a href="#Table2">Table 2</a>.<p></td>
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
<td><p></p></td>
<td><p>Buyer’s fund/budget number: a string defined by the library</p></td>
<td><p>M</p></td>
<td><pre>  FundNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Buyer’s fund description: text</p></td>
<td><p>D</p></td>
<td><pre>  FundDescription</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Percentage to charge to this fund: a decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>  Percent</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Amount to charge to this fund, in order currency: a decimal number[5]</p></td>
<td><p>D</p></td>
<td><pre>  MonetaryAmount</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p></p></td>
<td><p>Buyer’s budget year</p></td>
<td><p>D</p></td>
<td><pre>  BudgetYear</pre></td>
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

*Example of a Retrieve Quotation Response XML payload using either the
SOAP or the HTTPS protocol and the POST method:*

```
<QuotationResponse version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/quotation">
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
    <QuotationNumber>Q12345</QuotationNumber>
    <ReferenceCoded>
      <ReferenceTypeCode>01</ReferenceTypeCode>
      <ReferenceNumber>001</ReferenceNumber>
      <ReferenceDateTime>20180520T1525</ReferenceDateTime>
    </ReferenceCoded>
    <QuotationType>05</QuotationType>
  </Header>
  <ItemDetail>
    <LineNumber>1</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780123456789</IDValue>
    </ProductIdentifier>
    <OrderQuantity>3</OrderQuantity>
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
</QuotationResponse>
```

*Example of a Retrieve Quotation Response JSON payload using either the
SOAP or the HTTPS protocol and the POST method:*

```
{"QuotationResponse": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/quotation",
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
    "QuotationNumber": "Q12345"
    "ReferenceCoded": {
      "ReferenceTypeCode": "01",
      "ReferenceNumber": "001",
      "ReferenceDateTime": "20180520T1525"
    },
    "QuotationType": "05"
  },
  "ItemDetail": [
    {
      "LineNumber": 1,
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780123456789"
      },
      "OrderQuantity": 3,
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

#### Table 1: <a name="Table1"></a>Supplier item availability codes

These codes are for use in <a href="#SupplierAvailabilityCode">SupplierAvailabilityCode</a>.
Most of these codes would not be expected to be used in a Quotation.

| **Code value** | **Description**                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- |
| 10             | Not yet available - reason may be provided by publisher product availability code and/or publishing status code                                    |
| 20             | Available - further information on the precise nature of the availability should normally be provided by publisher product availability code       |
| 21             | Available - from stock - no additional availability information would normally be provided                                                         |
| 23             | Available - manufactured on demand                                                                                                                 |
| 30             | Temporarily unavailable - reason may be provided by publisher product availability code                                                            |
| 31             | Temporarily unavailable due to stock taking                                                                                                        |
| 40             | Not available - if due to a supply chain issue, the reason should be provided by publisher product availability code and/or publishing status code |
| 41             | Not available - publisher address unknown                                                                                                          |
| 42             | Not available - publisher no longer trading                                                                                                        |
| 43             | Not available - rights restricted                                                                                                                  |
| 44             | Not available in pack/set form - only available singly                                                                                             |
| 80             | Sold - second-hand or antiquarian item                                                                                                             |
| 90             | Availability uncertain - no further information                                                                                                    |
| 91             | Availability uncertain - item not known / identifier not recognised                                                                                |
| 92             | Availability uncertain - apply to customer services                                                                                                |

#### <a name="Table2"></a>Table 2: Values for <ProcessingInstructionCode> element**

This table combines suggestions from BISAC committee members with
existing values from BIC TRADACOMS and EDItEUR EDIFACT formats. Wherever
it makes sense, each positive instruction code has a negative “Do
not...” counterpart, so that exceptions to an agreed processing
package can be specified. This is reflected in the list of values below.
The EDIFACT equivalent codes are shown purely for reference and must not
be used in `<ProcessingInstructionCode>`.


| **Description** |             **Code**                 |                      **Code**                        | **EDIFACT** |       |
| --------------- | ------------------------------------ | ---------------------------------------------------- | ----------- | ----- |
|                 |       **Processing applied**         |         **Processing cancelled / not applied**       |             |       |
| No processing / servicing    |                         | *NoProcessing*                                       |             | *NS*  |
| Supplier-applied copy number (for single copy only)  | *AppliedCopyNumber*     | *NoAppliedCopyNumber*        | *BB*        | *BBN* |
| Start of supplier-applied copy number range          | *AppliedCopyNumberFrom* |                              |             |       |
| End of supplier-applied copy number range            | *AppliedCopyNumberTo*   |                              |             |       |
| Apply security device, in accordance with separately agreed specification  | *SecurityDevice* | *NoSecurityDevice* |   *BS* | *BSN* |
| Plastic / Mylar jacket / sleeve, in accordance with separately agreed specification | *Jacket* | *NoJacket*   | *BJ*        | *BJN* |
| Apply spine label, in accordance with separately agreed specification | *SpineLabel* | *NoSpineLabel*         |             |       |
| Apply spine label, based upon string supplied in this copy detail | *SpineLabelString* |                      |             |       |
| Apply pocket, in accordance with separately agreed specification | *Pocket*     | *NoPocket*                  | *JK*        | *JKN* |
| Apply circulation card, in accordance with separately agreed specification | *CirculationCard* | *NoCirculationCard* |      |       |
| Apply date due slip, in accordance with separately agreed specification | *DateDueSlip* | *NoDateDueSlip*     |             |       |
| Apply binding/strengthening, in accordance with separately agreed specification | *Binding* | *NoBinding*     | *BI*        | *BIN* |
| Stamp, in accordance with separately agreed specification | *Stamp*             | *NoStamp*                   |             |       |
| Apply embossing, in accordance with separately agreed specification | *Embossing* | *NoEmbossing*             |             |       |
| Apply RFID chip, in accordance with separately agreed specification | *RFIDChip* | *NoRFIDChip*               |             |       |
| Provide audio/CD packaging, in accordance with separately agreed specification | *AudioPackaging* | *NoAudioPackaging* | *BP* | *BPN* |
| Apply classification, in accordance with separately agreed specification | *Classification* | *NoClassification* | *BC*     | *BCN* |
| Provide catalog record, in accordance with separately agreed specification | *Catalog* | *NoCatalog*          | *CA*        | *CAN* |
| Laminate (paperback) cover, in accordance with separately agreed specification | *Laminate* | *NoLaminate*    | *LA*        | *LAN* |
| Apply sewn flexi binding, in accordance with separately agreed specification | *SewnFlexi* | *NoSewnFlexi*    | *SF*        | *SFN* |
| Case bind paperback, in accordance with separately agreed specification | *CaseBind* | *NoCaseBind*           | *TR*        | *TRN* |
| Binding as supplied by publisher (cancels all processing related to binding, jacketing, reinforcement etc) | | *BindingAsSupplied* |      | *PF*  |
| Non-standard servicing – see instructions sent outside of EDI | *SeparateInstructions*  |                     | *NX*        |       |

#### Notes

<a name="Notes1"></a>1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

<a name="Notes2"></a>2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

<a name="Notes3"></a>3.  An ‘R’ in the right-most column means that the element is
    repeatable.

<a name="Notes4"></a>4.  This element meets a theoretical requirement to allow a monetary
    value to be specified as either a per copy value or a total value
    for all copies in this copy detail. In practice, only percentages
    are known to be needed to meet current requirements.

<a name="Notes5"></a>5.  This element meets a theoretical requirement to allow a monetary
    value to be specified as either a per copy value or a total value
    for all copies in this copy detail. In practice, only percentages
    are known to be needed to meet current requirements.
