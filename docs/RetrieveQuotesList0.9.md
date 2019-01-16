![BIC LOGO](media/bic.png)

### *Book Industry Communication*

### BIC Library Web Services API Standards

# Retrieve Quotes List

Version 0.9, 6 September 2018

**This document:** http://www.bic.org.uk/files/pdfs/BICLWSQuotesList-V0.9.pdf  
**XML schema:** http://www.bic.org.uk/files/xml/BICLWSQuotesList-V0.9.xsd  
**WSDL file:** http://www.bic.org.uk/files/xml/BICLWSQuotesListSOAP-V0.9.wsdl  
**XML namespace:** http://www.bic.org.uk/librarywebservices/quotesList  
**Latest next review date:** 1 July 2020

This document specifies in human-readable form the Request and Response
formats for the BIC Library Web Services Retrieve Quotes List API.

The use of this document is subject to license terms and conditions that
can be found at <http://www.bic.org.uk/bicstandardslicence.pdf>.

A Retrieve Quotes List Request may be implemented using either SOAP or
the basic HTTPS protocol[\[1\]](#Notes1) and POST method. The payload of a Retrieve
Quotes List Request may be formatted either as an XML document or as an
equivalent JSON document.

The same Response format options (payload in XML or JSON) will apply to
both basic HTTPS and SOAP exchanges.

The complete specification of the Retrieve Quotes List Request/Response
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

There is a need for buyers, including EDI users, to ensure that they are
aware of all quotes prepared by a particular supplier. This web service
enables a buyer to retrieve a list of quotes, selected by date-of-issue
range or by quotation number pattern. Having retrieved such a list, a
buyer may then retrieve individual quotes using the BIC Library Web
Services Retrieve Quotation API.

A typical use of the Retrieve Quotes List API might involve the
following sequence:

1.  The supplier prepares quotations in accordance with the library’s
    instructions and uploads these to their web service.

2.  The library uses the Retrieve Quotes List API to retrieve a list of
    quotations prepared by the supplier.

3.  The library uses the Retrieve Quotation API one or more times to
    retrieve quotations from the supplier’s web service.

4.  The library processes each quotation and, if appropriate, uses the
    Order Request and Response API to confirm the order with the
    supplier.

This service will also enable an aggregation service to reconcile their
system with those of their data suppliers, to check for missing
documents and changes made outside their system.

### REQUEST

Requests should include an XML or JSON document as specified below as
the body of a request message.

**XML document encoding begins:** `<QuotesListRequest version="0.9">...`

**JSON document encoding begins:** `{ "QuotesListRequest": { "version": "0.9"...`

<table>
<tbody>
<tr valign="top">
  <th></th>
  <th>Description</th>
  <th><a href="#Notes2">[2]</a></th>
  <th>XML tag</th>
  <th><a href="#Notes3">[3]</a></th>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>A unique identifier for the sender of the request. An alphanumeric string not containing spaces or punctuation</p></td>
<td><p>D</p></td>
<td>
<pre>ClientID</pre>
</td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>A password to further authenticate the sender of the request</p></td>
<td><p>D</p></td>
<td>
<pre>ClientPassword</pre>
</td>
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
<td><p>References. If included, must contain a reference number or a reference date-time or both.</p></td>
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
<td></td>
<td><p>Reference</p></td>
<td><p>D</p></td>
<td><pre>  ReferenceNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td></td>
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
<td><p>8</p></td>
<td><p>Start date of the period for which the list of quotations prepared in that period is requested. – YYYYMDD</p></td>
<td><p>D</p></td>
<td><pre>PeriodStartDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>9</p></td>
<td><p>End date of the period for which the list of quotations prepared in that period is requested. – YYYYMMDD</p></td>
<td><p>D</p></td>
<td><pre>PeriodEndDate</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>10</p></td>
<td><p>Quotation number pattern to be matched. Use a regular expression that conforms to <a href="http://www.w3.org/TR/xmlschema11-2/#regexs">Appendix G of W3C XML Schema Definition Language (XSD) 1.1 Part 2: Datatypes</a>.</p></td>
<td><p>D</p></td>
<td><pre>ReferenceNumberPattern</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Retrieve Quotes List Request XML payload using either the
SOAP or the HTTPS protocol and the POST method, in which the request is
for all quotes issued from 1 April 2018 onwards:*

```
<QuotesListRequest version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/quotesList">
  <AccountIdentifier>
    <AccountIDType>01</AccountIDType>
    <IDValue>12345</IDValue>
  </AccountIdentifier>
  <RequestNumber>001</RequestNumber>
  <IssueDateTime>20180422T1525</IssueDateTime>
  <PeriodStartDate>20180401</PeriodStartDate>
</QuotesListRequest>
```

*JSON equivalent of the above payload:*

```
{
  "QuotesListRequest": {
    "version": "0.9",
    "xmlns": "http://www.bic.org.uk/librarywebservices/quotesList",
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

*Example of a Retrieve Quotes List Request XML payload using either the
SOAP or the HTTPS protocol and the POST method, in which the request is
for all quotes with numbers that match the regular expression pattern
‘01020\\d+’ (i.e. numbers beginning ‘01020’):*

```
<QuotesListRequest version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/quotesList">
  <AccountIdentifier>
    <AccountIDType>01</AccountIDType>
    <IDValue>12345</IDValue>
  </AccountIdentifier>
  <RequestNumber>001</RequestNumber>
  <IssueDateTime>20180422T1525</IssueDateTime>
  <ReferenceNumberPattern>01020\\d+</ReferenceNumberPattern>
</QuotesListRequest>
```

*JSON equivalent of the above payload:*

```
{
  "QuotesListRequest": {
    "version": "0.9",
    "xmlns": "http://www.bic.org.uk/librarywebservices/quotesList",
    "AccountIdentifier": {
      "AccountIDType": "01",
      "IDValue": "12345"
    },
    "RequestNumber": "001",
    "IssueDateTime": "20180422T1525",
    "ReferenceNumberPattern": "01020\\\\d+"
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

**XML document encoding begins:** `<QuotesListResponse version="0.9">...`

**JSON document encoding begins:** `{ "QuotesListResponse": { "version": "0.9"...`

<table>
<tbody>
<tr valign="top">
  <th></th>
  <th>Description</th>
  <th><a href="#Notes2">[2]</a></th>
  <th>XML tag</th>
  <th><a href="#Notes3">[3]</a></th>
</tr>
<tr valign="top">
<td colspan="5"><p><h4>Header</h4</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p><strong>Response payload header</strong></p></td>
<td><p><strong>M</strong></p></td>
<td><p><strong>Header.</strong></p></td>
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
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td><p>Reference type</p>
<ul><li><em>01</em>&nbsp;&nbsp;Number or date/time of associated quotes list request</li>
<li><em>16</em>&nbsp;&nbsp;Contract reference</li>
<li><em>35</em>&nbsp;&nbsp;Library’s supplier reference</li>
<li><em>36</em>&nbsp;&nbsp;Supplier’s library customer reference</li></ul></td>
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
<td><p>Response type code. Suggested code values:</p>
<ul><li><em>01</em>&nbsp;&nbsp;Service unavailable</li>
<li><em>02</em>&nbsp;&nbsp;Invalid ClientID or ClientPassword</li>
<li><em>03</em>&nbsp;&nbsp;Server unable to process request – a reason should normally be given as a free text description – see below</li>
<li><em>16</em>&nbsp;&nbsp;Invalid or unknown account, supplier or ship-to party identifier</li>
<li><em>17</em>&nbsp;&nbsp;Invalid period start or end date</li>
<li><em>18</em>&nbsp;&nbsp;Server unable to process request - specified range too large</li>
<li><em>19</em>&nbsp;&nbsp;Server unable to process request – unable to contact supplier</li></ul></td>
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
<td><p><strong>Details of all quotations that meet the selection criteria in the request. Mandatory unless the header reports a condition that prevents any response</strong></p></td>
<td><p><strong>D</strong></p></td>
<td><p><strong>ItemDetail.</strong></p></td>
<td><p><strong>R</strong></p></td>
</tr>
<tr valign="top">
<td><p>1</p></td>
<td><p>Quotes list response item line number</p></td>
<td><p>D</p></td>
<td><pre>LineNumber</pre></td>
<td></td>
</tr>
<tr valign="top">
<td><p>2</p></td>
<td><p>Document references. It is mandatory to include the supplier’s quotation reference in all items. If the quotation has already resulted in orders, one or more buyer’s order references must be included.</p></td>
<td><p>M</p></td>
<td><pre>ReferenceCoded</pre></td>
<td><p>R</p></td>
</tr>
<tr valign="top">
<td></td>
<td>Reference type code<br />
<em>11</em>&nbsp;&nbsp;Buyer’s order reference<br />
<em>29</em>&nbsp;&nbsp;Supplier’s quotation reference</td>
<td></td>
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
<td><p>Total number of lines on this quotation</p></td>
<td><p>M</p></td>
<td><pre>NumberOfLines</pre></td>
<td></td>
</tr>
</tbody>
</table>

*Example of a Retrieve Quotes List Response XML payload using either the
SOAP or the HTTPS protocol and the POST method:*

```
<QuotesListResponse version="0.9" xmlns="http://www.bic.org.uk/librarywebservices/quotesList">
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
      <ReferenceTypeCode>29</ReferenceTypeCode>
      <ReferenceNumber>Q12345</ReferenceNumber>
      <ReferenceDateTime>20180409</ReferenceDateTime>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>01020304</ReferenceNumber>
    </ReferenceCoded>
    <ReferenceCoded>
      <ReferenceTypeCode>11</ReferenceTypeCode>
      <ReferenceNumber>01020305</ReferenceNumber>
    </ReferenceCoded>
    <NumberOfLines>10</NumberOfLines>
  </ItemDetail>
  <ItemDetail>
    <ReferenceCoded>
      <ReferenceTypeCode>29</ReferenceTypeCode>
      <ReferenceNumber>Q12346</ReferenceNumber>
      <ReferenceDateTime>20180419</ReferenceDateTime>
    </ReferenceCoded>
    <NumberOfLines>8</NumberOfLines>
  </ItemDetail>
</QuotesListResponse>
```

*Example of a Retrieve Quotes List Response JSON payload using either the
SOAP or the HTTPS protocol and the POST method:*

```
{"QuotesListResponse": {
  "version": "0.9",
  "xmlns": "http://www.bic.org.uk/librarywebservices/quotesList",
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
          "ReferenceTypeCode": "29",
          "ReferenceNumber": "Q12345",
          "ReferenceDateTime": "20180409"
        },
        {
          "ReferenceTypeCode": "11",
          "ReferenceNumber": "1020304"
        },
        {
          "ReferenceTypeCode": "11",
          "ReferenceNumber": "1020305"
        }
      ],
      "NumberOfLines": 10
    },
    {
      "ReferenceCoded": {
        "ReferenceTypeCode": "29",
        "ReferenceNumber": "Q12346",
        "ReferenceDateTime": "20180419"
      },
      "NumberOfLines": 8
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
