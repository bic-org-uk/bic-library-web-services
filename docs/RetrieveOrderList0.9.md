![BIC LOGO](assets/bic-logo.png)

### *Book Industry Communication*

### BIC Library Web Services API Standards

# Retrieve Order List

**Version 0.9, 28 September 2018**

**The published version of this document:** <http://www.bic.org.uk/files/pdfs/BICLWSOrderList-V0.9.pdf>  
**XML schema:** <http://www.bic.org.uk/files/xml/BICLWSOrderList-V0.9.xsd>  
**WSDL file:** <http://www.bic.org.uk/files/xml/BICLWSOrderListSOAP-V0.9.wsdl>  
**XML namespace:** http://www.bic.org.uk/librarywebservices/orderList  
**Next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve Order List API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

A Retrieve Order List Request may be implemented using either SOAP or
the basic HTTPS protocol[\[1\]](#Notes1) and POST method. The payload of a P\&A
Request may be formatted either as an XML document or as an equivalent
JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTP and SOAP exchanges.

The complete specification of the Retrieve Order List Request/Response
web service includes two machine-readable resources that are to be used
by implementers in conjunction with this document:

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

### Business requirements

There is a need for library buyers, including EDI users, to ensure that
they are aware of all orders received and not yet fulfilled by a
particular supplier. This web service enables a buyer to retrieve a list
of orders, selected by date-of-issue range. Having retrieved such a
list, a buyer may then follow up individual orders using other web
services or by manual enquiry.

This service will also enable an aggregation service to reconcile their
system with those of their data suppliers, to check for missing
documents and changes made outside their system.

### REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**XML document encoding begins:** `<OrderListRequest version="0.9">...`

**JSON document encoding begins:** `{ "OrderListRequest": { "version": "0.9"...`

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
<td><p>Account identifier. Mandatory in all order list requests.</p></td>
<td><p>M</p></td>
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
<td></td>
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
<td><p>Supplier to whom this request should be forwarded, if it is not addressed to the web service host (use only for requests sent to aggregation services).</p></td>
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
<td><p>7</p></td>
<td><p>Start date of the period for which the list of orders placed in that period is requested. – YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>PeriodStartDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>8</p></td>
<td><p>End date of the period for which the list of orders placed in that period is requested. – YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>PeriodEndDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>Order number pattern to be matched. Use a regular expression that conforms to <a href="http://www.w3.org/TR/xmlschema11-2/#regexs">Appendix G of W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes</a>.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceNumberPattern</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Order line status has changed or not. Used to request a list of orders in which either at least one order line status has changed, or no order line status has changed. If this element is included, the changed-after date must be specified (see line 11).</p>
<ul><li><em>00</em>&nbsp;&nbsp;Order status has not changed</li>
<li><em>01</em>&nbsp;&nbsp;Order status has changed</li></ul></td>
<td><p>D</p></td>
<td><pre>OrderStatusChanged</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>11</p></td>
<td><p>Date after which the order line status has changed or not, depending upon the OrderStatusChanged code. Mandatory if OrderStatusChanged is included, otherwise not used. – YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>ChangedAfterDate</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Retrieve Order List Request XML payload using either the
SOAP or the HTTP protocol and the HTTP POST method, in which the request
is for all orders issued from 1 April 2015 onwards:*

```
<OrderListRequest version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/orderList">
  <AccountIdentifier>
    <AccountIDType>01</AccountIDType>
    <IDValue>12345</IDValue>
  </AccountIdentifier>
  <RequestNumber>001</RequestNumber>
  <IssueDateTime>20180422T1525</IssueDateTime>
  <PeriodStartDate>20180401</PeriodStartDate>
</OrderListRequest>
```

*Equivalent JSON payload:*

```
{
  "OrderListRequest": {
    "version": "0.9",
    "xmlns": "http://www.bic.org.uk/librarywebservices/orderList",
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "IssueDateTime": "20180422T1525",
    "PeriodStartDate": "20180401"
  }
}
```

*Example of a Retrieve Order List Request XML payload using either the
SOAP or the HTTP protocol and the HTTP POST method, in which the request
is for all orders with order numbers that match the regular expression
pattern ‘01020\\d+’ (i.e. numbers beginning ‘01020’):*

```
<OrderListRequest version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/orderList">
  <AccountIdentifier>
    <AccountIDType>01</AccountIDType>
    <IDValue>12345</IDValue>
  </AccountIdentifier>
  <RequestNumber>001</RequestNumber>
  <IssueDateTime>20150422T1525</IssueDateTime>
  <ReferenceNumberPattern>01020\d+</ReferenceNumberPattern>
</OrderListRequest>
```

*Equivalent JSON payload:*

```
{
  "OrderListRequest": {
    "version": "0.9",
    "xmlns": "http://www.bic.org.uk/librarywebservices/orderList",
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "IssueDateTime": "20150422T1525",
    "ReferenceNumberPattern": "01020\\d+"
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

**XML document encoding begins:** `<OrderListResponse version="0.9">...`

**JSON document encoding begins:** `{ "OrderListResponse": { "version": "0.9"...`

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
<td colspan="5"><h4>Response header</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Response payload header</strong></p></td>
<td><p><strong>M</strong></p></td>
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
<td><p>Account identifier. Mandatory in all responses.</p></td>
<td><p>M</p></td>
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
<td></td>
<td><p>Account identifier for this request, using the specified scheme</p></td>
<td><p>M</p></td>
<td><pre>  IDValue</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>5</p></td>
<td><p>References: request number and/or date/time of request must be quoted if included in the request.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceCoded</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><ul><li><em>01</em>&nbsp;&nbsp;Number or date/time of associated order list request</li></ul></td>
<td><p>M</p></td>
<td><pre>  ReferenceTypeCode</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference number / string</p></td>
<td><p>M</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference date or date and time. Mandatory if an IssueDateTime is included in the request. See Header line 1 for format options.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>6</p></td>
<td><p>Supplier identifier (only included if specified in the request; mandatory if the response type code is ‘19’)</p></td>
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
<td><p>ID type name, only if Supplier ID type is proprietary</p></td>
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
<td><p>7</p></td>
<td><p>Response code, if there are exception conditions.</p></td>
<td><p>D</p></td>
<td><pre>ResponseCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td>Response type code. Suggested code values:<br />
<ul><li><em>01</em>&nbsp;&nbsp;Service unavailable</li>
<li><em>02</em>&nbsp;&nbsp;Invalid ClientID or ClientPassword</li>
<li><em>03</em>&nbsp;&nbsp;Server unable to process request – a reason should normally be given as a free text description – see below</li>
<li><em>16</em>&nbsp;&nbsp;Invalid or unknown account, supplier or ship-to party identifier</li>
<li><em>17</em>&nbsp;&nbsp;Invalid period start or end date</li>
<li><em>18</em>&nbsp;&nbsp;Server unable to process request - specified range too large</li>
<li><em>19</em>&nbsp;&nbsp;Server unable to process request – unable to contact supplier</li></ul>
</td>
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
<td colspan="5"><h4>Response detail</h4></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Details of all orders that meet the selection criteria in the request. Mandatory unless the header reports a condition that prevents any response</strong></p></td>
<td><p><strong>D</strong></p></td>
<td><p><strong>ItemDetail</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Order list response item line number</p></td>
<td><p>D</p></td>
<td><pre>LineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Document references. It is mandatory to include the buyer’s order reference in all items. If the order has been partly or completely fulfilled, one or more delivery note references must be included.</p></td>
<td><p>M</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type code</p>
<ul><li><em>11</em>&nbsp;&nbsp;Buyer’s order reference</li>
<li><em>23</em>&nbsp;&nbsp;Supplier’s order reference</li></ul></td>
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
<td><p>Reference date or date and time. See Header line 1 for format options.</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceDateTime</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>3</p></td>
<td><p>Total number of order lines on this order</p></td>
<td><p>M</p></td>
<td><pre>NumberOfLines</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>4</p></td>
<td><p>Total number of order lines not yet fulfilled on this order</p></td>
<td><p>M</p></td>
<td><pre>NumberOfOpenLines</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Retrieve Order List Response XML payload using either the
SOAP or the HTTP protocol and the HTTP POST method:*

```
<OrderListResponse version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/orderList">
  <Header>
    <IssueDateTime>20180422T1527</IssueDateTime>
    <SenderIdentifier>
      <SenderIDType>01</SenderIDType>
      <IDValue>XYZ</IDValue>
    </SenderIdentifier>
    <AccountIdentifier>
      <AccountIDType>01</AccountIDType>
      <IDValue>12345</IDValue>
    </AccountIdentifier>
  </Header>
  <ItemDetail>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>O1020304</ReferenceNumber>
      <ReferenceDateTime>20180409</ReferenceDateTime>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>23</ReferenceTypeCode>
      <ReferenceNumber>DN0123456</ReferenceNumber>
    </ReferenceCoded>
    <NumberOfLines>10</NumberOfLines>
    <NumberOfOpenLines>5</NumberOfOpenLines>
  </ItemDetail>
  <ItemDetail>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>O1020405</ReferenceNumber>
      <ReferenceDateTime>20180419</ReferenceDateTime>
    </ReferenceCoded>
    <NumberOfLines>8</NumberOfLines>
    <NumberOfOpenLines>8</NumberOfOpenLines>
  </ItemDetail>
</OrderListResponse>
```

*Example of a Retrieve Order List Response JSON payload using either the
SOAP or the HTTP protocol and the HTTP POST method:*

```
{
"OrderListResponse": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/orderList",
  "Header": {
    "IssueDateTime": "20180422T1527",
    "SenderIdentifier": {
      "SenderIDType": "01",
      "IDValue": "XYZ"
    },
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    }
  },
  "ItemDetail": [
    {
      "ReferenceCoded": [
        {
          "ReferenceTypeCode": "11",
          "ReferenceNumber": "O1020304",
          "ReferenceDateTime": "20180409"
        },
        {
          "ReferenceTypeCode": "23",
          "ReferenceNumber": "DN0123456"
        }
      ],
      "NumberOfLines": 10,
      "NumberOfOpenLines": 5
    },
    {
      "ReferenceCoded": {
        "ReferenceTypeCode": "11",
        "ReferenceNumber": "O1020405",
        "ReferenceDateTime": "20180419"
      },
      "NumberOfLines": 8,
      "NumberOfOpenLines": 8
    }
  ]
}}
```

#### Notes

<a name="Notes1"></a>1.  Throughout the term ‘HTTPS protocol’ is to be interpreted as a
    secure internet protocol that is implemented either at the
    application layer (i.e. HTTPS) or at the transport layer (e.g.
    SSL/TLS).

<a name="Notes2"></a>2.  In the third column “M” means mandatory and “D” means dependent upon
    the business or message context.

<a name="Notes3"></a>3.  An ‘R’ in the right-most column means that the element is
    repeatable.
