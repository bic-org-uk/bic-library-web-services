![BIC LOGO](assets/bic-logo.png)

### *Book Industry Communication

### BIC Library Web Services Standards

# Order Status Enquiry and Report

**Version 0.9, 28 September 2018**

**The published version of this document:** <http://www.bic.org.uk/files/pdfs/BICLWSOrderStatusEnquiryReport-V0.9.pdf>  
**XML schema:** <http://www.bic.org.uk/files/xml/BICLWSOrderStatusEnquiryReport-V0.9.xsd>  
**WSDL file:** <http://www.bic.org.uk/files/xml/BICLWSOrderStatusEnquiryReportSOAP-V0.9.wsdl>  
**XML namespace:** http://www.bic.org.uk/librarywebservices/orderStatus  
**Next review date:**1 July 2020

This document specifies in human-readable form the request and response
formats for the BIC Library Web Services Order Enquiry and Report API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

An Order Status Enquiry (request) may be implemented using either SOAP or
the basic HTTPS protocol\[1\](#Notes1) and POST method. The payload of an Order
Status Enquiry may be formatted either as an XML document or as an
equivalent JSON document.

The same Order Status Report format options (payload in XML or JSON)
will apply to both basic HTTPS and SOAP exchanges.

The complete specification of the Order Status Enquiry and Report web
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

These formats are closely based upon the EDItX Library Order Status
Enquiry and Library Order Status Report formats developed by EDItEUR.

**Business requirements**

This API can be used in one of two ways.

The first is as an order chaser/status update to find the current
progress of an order. This web service allows the client to find out if
the supplier is awaiting stock, or has already shipped the order,
allowing them to update their customer in turn. When the order has been
shipped it allows the client to find out parcel details, including
tracking details when applicable.

The second, in combination with the Order List API, is as a
synchronisation process. By using the Order List API the library
management system can get the order references of orders that are not
yet logged within it. A subsequent call to the order status
request/report service allows the system to fill in the detail of those
orders. This is useful when customers use several channels to raise
orders with the supplier. This makes management of orders easier for the
buyer as all their orders are in one place, but also by being aware of
what has been ordered via other routes they can avoid potential over
ordering.

### REQUEST (ORDER STATUS ENQUIRY)

Requests should include a request document as the body of a request
message. Each request must contain a header followed by one or more
order detail elements. Each order detail element must contain at least
one order reference, an indicator of whether the request relates to the
whole order or to individual lines and, only if the latter, one or more
item detail elements relating to individual order lines.

**XML document encoding begins:** `<OrderStatusEnquiry version="0.9">...`

**JSON document encoding begins:** `{ "OrderStatusEnquiry": { "version": "0.9"...`

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
<td>1</td>
<td><p>A unique identifier for the sender of the request. An alphanumeric string not containing spaces or punctuation</p></td>
<td><p>D</p></td>
<td><pre>ClientID</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>2</td>
<td><p>A password to further authenticate the sender of the request</p></td>
<td><p>D</p></td>
<td><pre>ClientPassword</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>3</td>
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
<td>4</td>
<td><p>Order status request number</p></td>
<td><p>D</p></td>
<td><pre>  RequestNumber</pre></td>
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
<td>6</td>
<td><p>Supplier to whom this order status enquiry applies, if it is not addressed to the web service host (use only for requests sent to aggregators).</p></td>
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
<td>7</td>
<td><p>Order references. Must contain a reference number or a reference date or both for a single order.</p></td>
<td><p>M</p></td>
<td><pre>ReferenceCoded</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>11</em>&nbsp;&nbsp;Buyer’s order reference</li>
<li><em>23</em>&nbsp;&nbsp;Supplier’s order reference</li>
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
<td><p>Reference date or date and time. See Header line 5 for format options.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>8</td>
<td><p>Scope of the order status request. Mandatory.</p>
<ul><li><em>01</em>&nbsp;&nbsp;WholeOrder</li>
<li><em>02</em>&nbsp;&nbsp;ItemList</li></ul></td>
<td><p>M</p></td>
<td><pre>RequestType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Request order item detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Item detail. Mandatory if the scope of the request order detail is an item list. Must not be included if the scope of the request order detail is the whole order</strong></p></td>
<td><p>D</p></td>
<td><p><strong>ItemDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td>1</td>
<td><p>Order status enquiry line number</p></td>
<td><p>M</p></td>
<td><pre>LineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>2</td>
<td><p>EAN-13 product number (either this element or element 3 must be included)</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>3</td>
<td><p>Alternative product identifier</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td>R</td>
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
<td>4</td>
<td><p>Quantity ordered (may be required by trading partner agreement)</p></td>
<td><p>D</p></td>
<td><pre>OrderQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>5</td>
<td><p>Order line references If included, must contain a reference number or a reference date/time or both.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>12</em>&nbsp;&nbsp;Buyer’s unique order line reference</li>
<li><em>18</em>&nbsp;&nbsp;End customer order reference</li>
<li><em>23</em>&nbsp;&nbsp;Supplier’s order reference</li>
<li><em>33</em>&nbsp;&nbsp;Buyer’s internal supplier reference</li></ul></td>
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
<td><p>Reference date or date and time. See Header line 5 for format options.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of an Order Status Request XML payload using either the SOAP or
the HTTP protocol and the HTTP POST method:*

```
<OrderStatusEnquiry version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/orderStatus">
  <Header>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
    <RequestNumber>001</RequestNumber>
    <IssueDateTime>20181120T1525</IssueDateTime>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>1012345</ReferenceNumber>
    </ReferenceCoded>
    <RequestType>02</RequestType>
  </Header>
  <ItemDetail>
    <LineNumber>1</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780123456789</IDValue>
    </ProductIdentifier>
    <ReferenceCoded>
      <ReferenceTypeCode>12</ReferenceTypeCode>
      <ReferenceNumber>5</ReferenceNumber>
    </ReferenceCoded>
  </ItemDetail>
  <ItemDetail>
    <LineNumber>2</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780987654321</IDValue>
    </ProductIdentifier>
    <ReferenceCoded>
      <ReferenceTypeCode>12</ReferenceTypeCode>
      <ReferenceNumber>6</ReferenceNumber>
    </ReferenceCoded>
  </ItemDetail>
</OrderStatusEnquiry>
```

*Example of an Order Status Request JSON payload using either the SOAP
or the HTTP protocol and the HTTP POST method:*

```
{"OrderStatusEnquiry": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/orderStatus",
  "Header": {
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "IssueDateTime": "20181120T1525",
    "ReferenceCoded": {
      "ReferenceTypeCode": "11",
      "ReferenceNumber": 1012345
    },
    "RequestType": "02"
  },
  "ItemDetail": [
    {
      "LineNumber": "1",
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780123456789"
      },
      "ReferenceCoded": {
        "ReferenceTypeCode": "12",
        "ReferenceNumber": "5"
      }
    },
    {
      "LineNumber": "2",
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780987654321"
      },
      "ReferenceCoded": {
        "ReferenceTypeCode": 12,
        "ReferenceNumber": "6"
      }
    }
  ]
}}
```

### RESPONSE (ORDER STATUS REPORT)

The Order Status Report will use the protocol corresponding to the Order
Status Enquiry. If the Enquiry uses the basic HTTP protocol, the Report
will be an XML document as specified below attached to a normal HTTP
header. If the Enquiry uses the SOAP protocol, the Report will contain a
SOAP response message whose body will contain the XML document specified
below.

**XML document encoding begins:** `<OrderStatusReport version="0.9">...`

**JSON document encoding begins:** `{ "OrderStatusReport": { "version": "0.9"...`

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
<td>1</td>
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
<td>2</td>
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
<td><p>D</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>3</td>
<td><p>Identification number / string of this response</p></td>
<td><p>D</p></td>
<td><pre>ResponseNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>4</td>
<td><p>Account identifier. Mandatory if included in request.</p></td>
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
<td>5</td>
<td><p>References: the request number and/or date/time of request must be quoted if included in the request; the order number must be quoted.</p></td>
<td><p>M</p></td>
<td><pre>ReferenceCoded</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Number or date/time of associated request</li>
<li><em>11</em>&nbsp;&nbsp;Order number or date/time specified in the request</li>
<li><em>23</em>&nbsp;&nbsp;Supplier’s order reference</li>
<li><em>29</em>&nbsp;&nbsp;Supplier’s quotation reference</li>
<li><em>37</em>&nbsp;&nbsp;Library’s additional order reference</p></ul></td>
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
<td><p>Reference date or date and time. Mandatory if an IssueDateTime is included in the request. See Header line 1 for allowed formats.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>6</td>
<td><p>Response code, if there are exception conditions, in which case this composite terminates the response.</p></td>
<td><p>D</p></td>
<td><pre>ResponseCoded</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Response type code. Suggested code values:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Service unavailable</li>
<li><em>02</em>&nbsp;&nbsp;Invalid ClientID or ClientPassword</li>
<li><em>03</em>&nbsp;&nbsp;Server unable to process request – a reason should normally be given as a free text description – see below</li>
<li><em>11</em>&nbsp;&nbsp;Invalid order reference</li>
<li><em>16</em>&nbsp;&nbsp;Invalid or unknown account or supplier identifier</li>
<li><em>19</em>&nbsp;&nbsp;Server unable to process request – unable to contact supplier</li>
<li><em>20</em>&nbsp;&nbsp;Order status request acknowledged – awaiting response from supplier</li>
<li><em>24</em>&nbsp;&nbsp;Request does not reference a unique order</li></ul></td>
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
<td>7</td>
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
<td>8</td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>9</td>
<td><p>Default currency of values given in the response. If omitted, ‘GBP’ is assumed.</p></td>
<td><p>D</p></td>
<td><pre>CurrencyCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Shipping detail <em>(if supplier has more than one warehouse, the default warehouse location from which goods have been / will be shipped; may be overridden at the item detail level – see response detail line 12)</em></p></td>
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
<td>R</td>
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
<td><p>11</p></td>
<td><p>Order status – mandatory unless there is an exception condition.</p>
<ul><li><em>01</em>&nbsp;&nbsp;Order accepted – shipping</li>
<li><em>02</em>&nbsp;&nbsp;Order accepted – all backordered</li>
<li><em>03</em>&nbsp;&nbsp;Order accepted – see detail</li>
<li><em>04</em>&nbsp;&nbsp;Order accepted but cannot be processed normally – see order status message</li>
<li><em>05</em>&nbsp;&nbsp;Order not accepted – see detail</li>
<li><em>06</em>&nbsp;&nbsp;Order accepted – awaiting shipping authorization</li></ul></td>
<td><p>D</p></td>
<td><pre>OrderStatus</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><p>Order status message. Mandatory if the order status is ‘04’.</p></td>
<td><p>D</p></td>
<td><pre>OrderStatusMessage</pre></td>
<td></td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Report order item detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Responses for each item included in the order status request, or all items if the scope of the request is the whole order. Mandatory in each order status report unless the response type code at that level indicates an exception condition</strong></p></td>
<td><p>D</p></td>
<td><p><strong>ItemDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td>1</td>
<td><p>Line item number</p></td>
<td><p>M</p></td>
<td><pre>LineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>2</td>
<td><p>EAN-13 product number (mandatory unless trading partners have agreed to use an alternative product identifier)</p></td>
<td><p>D</p></td>
<td><pre>EAN13</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>3</td>
<td><p>Alternative product identifier – for details see the corresponding line in Request detail</p></td>
<td><p>D</p></td>
<td><pre>ProductIdentifier</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td>4</td>
<td><p>Item description</p></td>
<td><p>D</p></td>
<td><pre>ItemDescription</pre></td>
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
<td>5</td>
<td><p>Quantity ordered</p></td>
<td><p>M</p></td>
<td><pre>OrderQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>6</td>
<td><p>Order line references If included, must contain a reference number or a reference date or both.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Enquiry line number</li>
<li><em>12</em>&nbsp;&nbsp;Buyer’s unique order line reference</li>
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
<td><p>Reference date or date and time. See Report Header line 1 for format options.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>7</td>
<td><p>Order priority code: a string defined by trading partner agreement</p></td>
<td><p>D</p></td>
<td><pre>OrderPriorityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>8</td>
<td><p>For digital items, the technical protection method(s) applied as a condition of sale at the supplier’s price identifier. Repeatable – use ONIX code list 144.</p></td>
<td><p>D</p></td>
<td><pre>EpubTechnicalProtection</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td>9</td>
<td><p>Usage constraint(s) associated with the supplier’s identified price. Repeatable</p></td>
<td><p>D</p></td>
<td><pre>PriceConstraint</pre></td>
<td>R</td>
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
<td>R</td>
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
<td>R</td>
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
<td>R</td>
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
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of quantity – use ONIX code list 168.</p></td>
<td><p>M</p></td>
<td><pre>  PriceConditionQuantityType</pre></td>
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
<td>R</td>
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
<td><p>M</p></td>
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
<td><p>M</p></td>
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
<td><p>Total % discount applying to this price – decimal number between 0 and 100</p></td>
<td><p>D</p></td>
<td><pre>  DiscountPercentage</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><p>Order line response status code. Mandatory for each order line. See Table 1 for EDItEUR order line status codes that may be used in this context.</p></td>
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
<td><p>Shipping detail (used only if the shipment was or will be from a location other than the default specified in the header)</p></td>
<td><p>D</p></td>
<td><pre>ShippingFrom</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location – must include at least one identifier,</p>
a name, or both</td>
<td><p>M</p></td>
<td><pre>  Location</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Location identifier</p></td>
<td><p>D</p></td>
<td><pre>    LocationIdentifier</pre></td>
<td>R</td>
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
<td><p>15</p></td>
<td><p>Quantity already shipped or in process of shipping</p></td>
<td><p>D</p></td>
<td><pre>ShippedQuantity</pre></td>
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
<td><pre>CancelledQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><p>Item link to packaging list (repeatable)</p></td>
<td><p>D</p></td>
<td><pre>PackageReference</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Quantity</p></td>
<td><p>D</p></td>
<td><pre>  PackedQuantity</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Package number in list</p></td>
<td><p>M</p></td>
<td><pre>  PackageNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>19</p></td>
<td><p>Product availability status</p></td>
<td><p>D</p></td>
<td><pre>AvailabilityCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Supplier item availability code value. See <a href="#Table2">Table 2</a>.</p></td>
<td><p>M</p></td>
<td><pre>  SupplierAvailabilityCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Publisher / distributor product availability code value – see ONIX codelist 65. See also <a href="#Table3">Table 3</a> for codes most likely to be used in this context and their EDIFACT equivalents.</p></td>
<td><p>D</p></td>
<td><pre>  ProductAvailabilityCode</pre></td>
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
<td><p><strong></p>
</strong>20</td>
<td><p>Substitute product details <em>(for further description see corresponding details for the ordered product)</em>.</p></td>
<td><p>D</p></td>
<td><pre>Substitute</pre></td>
<td></td>
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
<td><p>Alternative product identifier</p></td>
<td><p>D</p></td>
<td><pre>  ProductIdentifier</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td><p>21</p></td>
<td><p>Message required for all or specific copies of this line item (absence of this element means “no message required”) (repeatable). The specified message is to be included in all documents relating to the order line.</p></td>
<td><p>D</p></td>
<td><pre>Message</pre></td>
<td>R</td>
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
<td>R</td>
</tr>
<tr valign="top">
<td colspan="5"><h4>Packaging Detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Packaging detail</strong></p></td>
<td><p><strong>D</strong></p></td>
<td><p><strong>PackageDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td>1</td>
<td><p>Package type</p></td>
<td><p>M</p></td>
<td><pre>PackageCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Scheme from which package type code is taken:</p>
<ul><li><em>01</em>&nbsp;&nbsp;EDItEUR international package code</li>
<li><em>02</em>&nbsp;&nbsp;UK book trade package code</li>
<li><em>03</em>&nbsp;&nbsp;US book trade package code</li></ul></td>
<td><p>M</p></td>
<td><pre>  PackageCodeType</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Package type code</p></td>
<td><p>M</p></td>
<td><pre>  PackageCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>2</td>
<td><p>Number of packages</p></td>
<td><p>M</p></td>
<td><pre>NumberOfPackages</pre></td>
<td></td>
</tr>
<tr valign="top">
<td>3</td>
<td><p>Package description (repeatable)</p></td>
<td><p>D</p></td>
<td><pre>Package</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Package number in list, required if package contents (as line items) are linked to the packages in which they are sent</p></td>
<td><p>D</p></td>
<td><pre>  PackageNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Package mark</p></td>
<td><p>M</p></td>
<td><pre>  PackageMark</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Type of package mark</p>
<ul><li><em>01</em>&nbsp;&nbsp;SSCC-18 standard</li>
<li><em>02</em>&nbsp;&nbsp;Carrier’s proprietary number</li></ul></td>
<td><p>M</p></td>
<td><pre>    PackageMarkTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Package mark value</p></td>
<td><p>M</p></td>
<td><pre>    PackageMarkValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Measurements / weight</p></td>
<td><p>D</p></td>
<td><pre>  Measure</pre></td>
<td>R</td>
</tr>
<tr valign="top">
<td></td>
<td><p>Measure type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Height (of cover of book)</li>
<li><em>02</em>&nbsp;&nbsp;Width (of cover of book)</li>
<li><em>03</em>&nbsp;&nbsp;Thickness (of spine of book)</li>
<li><em>04</em>&nbsp;&nbsp;Total weight of delivered line</li></ul></td>
<td><p>M</p></td>
<td><pre>    MeasureTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Measurement value (decimal number)</p></td>
<td><p>M</p></td>
<td><pre>    Measurement</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Measure unit</p>
<ul><li><em>01</em>&nbsp;&nbsp;Centimetre</li>
<li><em>02</em>&nbsp;&nbsp;Millimetre</li>
<li><em>03</em>&nbsp;&nbsp;Inch</li>
<li><em>04</em>&nbsp;&nbsp;Gram</li>
<li><em>05</em>&nbsp;&nbsp;Kilogram</li>
<li><em>06</em>&nbsp;&nbsp;Ounce</li>
<li><em>07</em>&nbsp;&nbsp;Pound</li></ul></td>
<td><p>M</p></td>
<td><pre>    MeasureUnitCode</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of an Order Status Report XML payload using either the SOAP or
the HTTP protocol and the HTTP POST method:*

```
<OrderStatusReport version="0.9"
  xmlns="http://www.bic.org.uk/librarywebservices/orderStatus">
  <Header>
    <IssueDateTime>20181120T1526</IssueDateTime>
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
      <ReferenceDateTime>20181120T152500</ReferenceDateTime>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>1012345</ReferenceNumber>
    </ReferenceCoded>
    <OrderStatus>06</OrderStatus>
  </Header>
  <ItemDetail>
    <LineNumber>1</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780123456789</IDValue>
    </ProductIdentifier>
    <OrderQuantity>5</OrderQuantity>
    <ReferenceCoded>
      <ReferenceTypeCode>01</ReferenceTypeCode>
      <ReferenceNumber>1</ReferenceNumber>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>12</ReferenceTypeCode>
      <ReferenceNumber>5</ReferenceNumber>
    </ReferenceCoded>
    <Price>
      <MonetaryAmount>9.99</MonetaryAmount>
      <PriceQualifierCode>01</PriceQualifierCode>
    </Price>
    <OrderLineStatusCoded>
      <StatusCodeType>02</StatusCodeType>
      <StatusCode>AlreadyShipped</StatusCode>
    </OrderLineStatusCoded>
    <ShippedQuantity>5</ShippedQuantity>
  </ItemDetail>
  <ItemDetail>
    <LineNumber>2</LineNumber>
    <ProductIdentifier>
      <ProductIDType>03</ProductIDType>
      <IDValue>9780987654321</IDValue>
    </ProductIdentifier>
    <OrderQuantity>2</OrderQuantity>
    <ReferenceCoded>
      <ReferenceTypeCode>01</ReferenceTypeCode>
      <ReferenceNumber>2</ReferenceNumber>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>12</ReferenceTypeCode>
      <ReferenceNumber>6</ReferenceNumber>
    </ReferenceCoded>
    <Price>
      <MonetaryAmount>15.99</MonetaryAmount>
      <PriceQualifierCode>01</PriceQualifierCode>
    </Price>
    <OrderLineStatusCoded>
      <StatusCodeType>02</StatusCodeType>
      <StatusCode>AcceptedBackordered</StatusCode>
    </OrderLineStatusCoded>
    <BackorderedQuantity>2</BackorderedQuantity>
    <AvailabilityCoded>
      <PublisherAvailabilityCode>31</PublisherAvailabilityCode>
      <ExpectedShipDate>20181122</ExpectedShipDate>
    </AvailabilityCoded>
  </ItemDetail>
</OrderStatusReport>
```

*Example of an Order Status Report JSON payload using either the SOAP or
the HTTP protocol and the HTTP POST method:*

```
{"OrderStatusReport": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/orderStatus",
  "Header": {
    "IssueDateTime": "20181120T1526",
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
        "ReferenceDateTime": "20181120T152500"
      },
      {
        "ReferenceTypeCode": "11",
        "ReferenceNumber": "1012345"
      }
    ],
    "OrderStatus": "06"
  },
  "ItemDetail": [
    {
      "LineNumber": "1",
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780123456789"
      },
      "OrderQuantity": 5,
      "ReferenceCoded": [
        {
          "ReferenceTypeCode": "01",
          "ReferenceNumber": "1"
        },
        {
          "ReferenceTypeCode": "12",
          "ReferenceNumber": "5"
        }
      ],
      "Price": {
        "MonetaryAmount": 9.99,
        "PriceQualifierCode": "01"
      }
      "OrderLineStatusCoded": {
        "StatusCodeType": "02",
        "StatusCode": "AlreadyShipped"
      },
      "ShippedQuantity": 5
    },
    {
      "LineNumber": "2",
      "ProductIdentifier": {
        "ProductIDType": "03",
        "IDValue": "9780987654321"
      },
      "OrderQuantity": 2,
      "ReferenceCoded": [
        {
          "ReferenceTypeCode": "01",
          "ReferenceNumber": "2"
        },
        {
          "ReferenceTypeCode": "12",
          "ReferenceNumber": "6"
        }
      ],
      "Price": {
        "MonetaryAmount": 15.99,
        "PriceQualifierCode": "01"
      }
      "OrderLineStatusCoded": {
        "StatusCodeType": "02",
        "StatusCode": "AcceptedBackordered"
      },
      "BackorderedQuantity": 2,
      "AvailabilityCoded": {
        "PublisherAvailabilityCode": "31",
        "ExpectedShipDate": "20181122"
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
<td><p><strong>Equivalent</p>
EDIFACT code<br />
(List 12B)</strong></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><em>AcceptedBackordered</em></td>
<td><p>Order line accepted as backorder (“due”) (Order Response only)</p></td>
<td><p>400</p></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><em>AcceptedPartShippingPartBackordered</em></td>
<td><p>Order line accepted, part in process for immediate shipping, part backordered (Order Response only)</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><em>AcceptedPartShippingPartCanceled</em></td>
<td><p>Order line accepted, part in process for immediate shipping, part cancelled (Order Response only)</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><em>AcceptedShipping</em></td>
<td><p>Order line accepted, all in process for immediate shipping (Order Response only)</p></td>
<td><p>100</p></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><em>AlreadyShipped</em></td>
<td><p>Order line already shipped or in process for shipping</p></td>
<td><p>800</p></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><em>BackorderedAwaitingMinimumOrder</em></td>
<td><p>Backordered until minimum order quantity or value is reached</p></td>
<td><p>103 / 403</p></td>
</tr>
<tr valign="top">
<td><p>7</p></td>
<td><em>BackorderedAwaitingReceipt</em></td>
<td><p>Backordered and has been despatched from our supplier, awaiting receipt</p></td>
<td><p>404</p></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><em>BackorderedAwaitingSupply</em></td>
<td><p>Backordered – awaiting supply</p></td>
<td><p>400</p></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><em>BackorderedChasingSupplier</em></td>
<td><p>Order line has been chased to our supplier</p></td>
<td><p>413</p></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><em>BackorderedDateChange</em></td>
<td><p>Backordered, note change to expected availability date</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><em>BackorderedOnOrderFromOverseas</em></td>
<td><p>Backordered – has been ordered from overseas supplier</p></td>
<td><p>402</p></td>
</tr>
<tr valign="top">
<td><p>12</p></td>
<td><em>BackorderedPriceChange</em></td>
<td><p>Backordered, note price change</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>13</p></td>
<td><em>BackorderedTitleChange</em></td>
<td><p>Backordered, note title change (NYP only)</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>14</p></td>
<td><em>BackorderedStockTaking*</em></td>
<td><p>Backordered – currently unavailable due to stock taking</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>15</p></td>
<td><em>CanceledByBuyer</em></td>
<td><p>Order line cancelled at buyer’s request</p></td>
<td><p>222</p></td>
</tr>
<tr valign="top">
<td><p>16</p></td>
<td><em>CanceledCannotSupply</em></td>
<td><p>Order line cancelled, item cannot be supplied (eg out of print)</p></td>
<td><p>223</p></td>
</tr>
<tr valign="top">
<td><p>17</p></td>
<td><em>CanceledDiscountQuery</em></td>
<td><p>Discount query: order line cancelled</p></td>
<td><p>202</p></td>
</tr>
<tr valign="top">
<td><p>18</p></td>
<td><em>CanceledDuplicateOrder</em></td>
<td><p>Order appears to be a duplicate: order line cancelled</p></td>
<td><p>208</p></td>
</tr>
<tr valign="top">
<td><p>19</p></td>
<td><em>CanceledInvalid</em></td>
<td><p>Ordered item cannot be recognized</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>20</p></td>
<td><em>CanceledMinimumOrderReq</em></td>
<td><p>Minimum order value required: order line cancelled</p></td>
<td><p>203</p></td>
</tr>
<tr valign="top">
<td><p>21</p></td>
<td><em>CanceledOutOfTime</em></td>
<td><p>Order line cancelled, past cancelation date</p></td>
<td><p>221</p></td>
</tr>
<tr valign="top">
<td><p>22</p></td>
<td><em>CanceledPriceQuery</em></td>
<td><p>Price query: order line cancelled</p></td>
<td><p>201</p></td>
</tr>
<tr valign="top">
<td><p>23</p></td>
<td><em>CanceledPriceIdentifierMismatch*</em></td>
<td><p>Price identifier invalid or doesn’t match price amount: order line cancelled</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>24</p></td>
<td><em>CanceledPromotionInvalid</em></td>
<td><p>Promotional deal invalid or expired: order line cancelled</p></td>
<td><p>205 / 206</p></td>
</tr>
<tr valign="top">
<td><p>25</p></td>
<td><em>CanceledSubstOffered</em></td>
<td><p>Order line cancelled, substitute product is offered for separate order</p></td>
<td><p>210</p></td>
</tr>
<tr valign="top">
<td><p>26</p></td>
<td><em>CanceledTryOtherLocation</em></td>
<td><p>Order line cancelled, but could be supplied at a Ship From location other than that specified by the buyer: must be accompanied by the identity of the other location</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>27</p></td>
<td><em>CanceledUnknown*</em></td>
<td><p>Order line cancelled – unknown product or source of supply</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>28</p></td>
<td><em>CanceledRightsRestricted*</em></td>
<td><p>Order line cancelled –cannot supply to customer due to rights restrictions</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>29</p></td>
<td><em>CanceledCannotShipByRequestedDate*</em></td>
<td><p>Order line cancelled – cannot ship by requested “ship by” date</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>30</p></td>
<td><em>CanceledSold*</em></td>
<td><p>Order line cancelled – second-hand or antiquarian item already sold</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>31</p></td>
<td><em>CanceledStockTaking*</em></td>
<td><p>Order line cancelled – currently unavailable due to stock taking</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>32</p></td>
<td><em>HeldAccountStopped</em></td>
<td><p>Buyer’s account is temporarily stopped: product availability is reported as usual, but order will not be released until account is cleared</p></td>
<td><p>500</p></td>
</tr>
<tr valign="top">
<td><p>33</p></td>
<td><em>HeldAwaitingBuyerInstruction</em></td>
<td><p>Awaiting instruction from buyer: order line held</p></td>
<td><p>412</p></td>
</tr>
<tr valign="top">
<td><p>34</p></td>
<td><em>HeldDiscountQuery</em></td>
<td><p>Discount query: order line held awaiting customer response</p></td>
<td><p>102</p></td>
</tr>
<tr valign="top">
<td><p>35</p></td>
<td><em>HeldFirmOrderRequired</em></td>
<td><p>Can only be supplied against firm order: order line held awaiting customer response</p></td>
<td><p>104</p></td>
</tr>
<tr valign="top">
<td><p>36</p></td>
<td><em>HeldMinimumOrderReq</em></td>
<td><p>Minimum order value required: order line held</p></td>
<td><p>103</p></td>
</tr>
<tr valign="top">
<td><p>37</p></td>
<td><em>HeldPriceQuery</em></td>
<td><p>Price query: order line held awaiting customer response, usually because difference between order price and actual price exceeds agreed tolerance</p></td>
<td><p>903</p></td>
</tr>
<tr valign="top">
<td><p>38</p></td>
<td><em>HeldPriceIdentifierMismatch*</em></td>
<td><p>Price identifier invalid or doesn’t match price amount: order line held awaiting customer response.</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>39</p></td>
<td><em>HeldPromotionInvalid</em></td>
<td><p>Promotional deal invalid or expired: order line held awaiting customer response</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>40</p></td>
<td><em>HeldStockTaking*</em></td>
<td><p>Order line held – currently unavailable due to stock taking</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>41</p></td>
<td><em>NotFound</em></td>
<td><p>Order line not traced</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>42</p></td>
<td><em>NotOnBackorderFile</em></td>
<td><p>Response to order status enquiry or cancellation, for suppliers whose systems check enquiries or cancellations only against a backorder file. Where possible, the more informative responses NotFound and AlreadyShipped are preferred.</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>43</p></td>
<td><em>Processing</em></td>
<td><p>Ordered item(s) being processed by bookseller</p></td>
<td><p>410</p></td>
</tr>
<tr valign="top">
<td><p>44</p></td>
<td><em>ProcessingAwaitingBuyerInstruction</em></td>
<td><p>Ordered item(s) being processed by bookseller, awaiting buyer instruction</p></td>
<td><p>411</p></td>
</tr>
<tr valign="top">
<td><p>45</p></td>
<td><em>ReorderedSuppliedDamaged</em></td>
<td><p>Our supplier sent damaged item(s): reordered</p></td>
<td><p>407</p></td>
</tr>
<tr valign="top">
<td><p>46</p></td>
<td><em>ReorderedSuppliedImperfect</em></td>
<td><p>Our supplier sent imperfect item(s): reordered</p></td>
<td><p>408</p></td>
</tr>
<tr valign="top">
<td><p>47</p></td>
<td><em>ReorderedSuppliedShort</em></td>
<td><p>Our supplier sent short: reordered</p></td>
<td><p>406</p></td>
</tr>
<tr valign="top">
<td><p>48</p></td>
<td><em>ReorderedSupplierCannotTrace</em></td>
<td><p>Our supplier cannot trace order: reordered</p></td>
<td><p>409</p></td>
</tr>
<tr valign="top">
<td><p>49</p></td>
<td><em>ReorderedWrongItemSupplied</em></td>
<td><p>Our supplier sent wrong item(s): reordered</p></td>
<td><p>405</p></td>
</tr>
<tr valign="top">
<td><p>50</p></td>
<td><em>SubstBackordered</em></td>
<td><p>Substitute product backordered</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>51</p></td>
<td><em>SubstPartShippingPartBackordered</em></td>
<td><p>Substitute product part in process for immediate shipping, part backordered</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>52</p></td>
<td><em>SubstPartShippingPartCanceled</em></td>
<td><p>Substitute product part in process for immediate shipping, part cancelled</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>53</p></td>
<td><em>SubstShipping</em></td>
<td><p>Substitute product in process for immediate shipping</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>54</p></td>
<td><em>TemporaryHold</em></td>
<td><p>Order action not yet determined</p></td>
<td><p>999</p></td>
</tr>
<tr valign="top">
<td><p>55</p></td>
<td><em>HeldAdditionalServiceQuery</em></td>
<td><p>Order line held pending resolution of a query concerning an additional service requested for that order line</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>56</p></td>
<td><em>CanceledAdditionalServiceQuery</em></td>
<td><p>Order line cancelled due to a query concerning an additional service requested. The item should be re-ordered once the query has been resolved.</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>57</p></td>
<td><em>CanceledAccountStopped</em></td>
<td><p>Buyer’s account is temporarily stopped; the order line is cancelled, but product price and availability is reported.</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>58</p></td>
<td><em>AcceptedReadyForActivation*</em></td>
<td><p>Ordered digital item is ready for activation by the consumer</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>59</p></td>
<td><em>AcceptedActivated*</em></td>
<td><p>Ordered digital item is activated and ready for use by the consumer</p></td>
<td></td>
</tr>
<tr valign="top">
<td><p>60</p></td>
<td><em>AwaitingAuthorizationToShip*</em></td>
<td><p>Awaiting authorisation to despatch</p></td>
<td></td>
</tr>
</tbody>
</table>

**Table 2<a name="Table2"></a>: Supplier item availability codes**

Used in SupplierAvailabilityCode. This code list is
derived from ONIX codelist 65 (subset shown in <a href="#Table3">Table 3</a>), but with
significant differences for code values 31 and above.

<table>
<thead>
<tr class="header">
<th><strong>Code value</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td><p>10</p></td>
<td><p>Not yet available - reason may be provided by publisher product availability code</p>
(e.g. 10, 11 or 12) and/or publishing status code (e.g. 02)</td>
</tr>
<tr valign="top">
<td><p>20</p></td>
<td><p>Available - further information on the precise nature of the availability may be provided by publisher/distributor product availability code (e.g. 20, 21, 22 or 23)</p></td>
</tr>
<tr valign="top">
<td><p>21</p></td>
<td><p>Available - from stock - no additional availability information would normally be provided</p></td>
</tr>
<tr valign="top">
<td><p>23</p></td>
<td><p>Available - manufactured on demand</p></td>
</tr>
<tr valign="top">
<td><p>30</p></td>
<td><p>Temporarily unavailable - reason may be provided by publisher product availability code</p></td>
</tr>
<tr valign="top">
<td><p>31</p></td>
<td><p>Temporarily unavailable due to stock taking</p></td>
</tr>
<tr valign="top">
<td><p>40</p></td>
<td><p>Not available - if due to a supply chain issue, the reason should be provided by publisher product availability code and/or publishing status code</p></td>
</tr>
<tr valign="top">
<td><p>41</p></td>
<td><p>Not available - publisher address unknown</p></td>
</tr>
<tr valign="top">
<td><p>42</p></td>
<td><p>Not available - publisher no longer trading</p></td>
</tr>
<tr valign="top">
<td><p>43</p></td>
<td><p>Not available - rights restricted</p></td>
</tr>
<tr valign="top">
<td><p>44</p></td>
<td><p>Not available in pack/set form - only available singly</p></td>
</tr>
<tr valign="top">
<td><p>80</p></td>
<td><p>Sold - second-hand or antiquarian item</p></td>
</tr>
<tr valign="top">
<td><p>90</p></td>
<td><p>Availability uncertain - no further information</p></td>
</tr>
<tr valign="top">
<td><p>91</p></td>
<td><p>Availability uncertain - item not known / identifier not recognised</p></td>
</tr>
<tr valign="top">
<td><p>92</p></td>
<td><p>Availability uncertain - apply to customer services</p></td>
</tr>
</tbody>
</table>

**Table 3<a name="Table3"></a>: EDItEUR product availability codes**

This table defines a subset of ONIX codelist 65 mostly likely to be used
in ProductAvailabilityCode. The equivalent EDIFACT
code (List 8B) is shown for information only, as the ONIX code must
always be
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

#### Notes

<a name="Notes1"></a>1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

<a name="Notes2"></a>2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

<a name="Notes3"></a>3.  An ‘R’ in the right-most column means that the element is
    repeatable.

