# Country-specific rules for the Netherlands in Peppol BIS 3 Invoice and CreditNote

*Version 1.0, 2021-08-30*

## 1 Introduction

This document defines a set of country-specific business rules for the Netherlands in Peppol BIS 3 Invoice and CreditNote.

Business rules are semantic rules that define whether a document is considered valid. This does not only apply to the structure, i.e. which fields are present and what type of data they contain, but also about their contents, e.g. which code lists are used, or whether a number may be negative, etc.

We shall refer to Peppol BIS 3 Invoice and CreditNote as 'Peppol BIS 3' for the remainder of this document.

We shall refer to business rules simply as ‘rules’ for the remainder of this document.

This version is a proposal, it is not a specification yet.

## 2 Definition and scope

As a basis, this document takes the rules as defined by the STPE in the NLCIUS (“Gebruiksinstructie voor de basisfactuur conform EN 16931-1”), and considers a number of them for inclusion in Peppol BIS 3. Some of these rules may need to be modified, while other rules may need to be dropped altogether.

The result is a subset of the NLCIUS rules that is added to Peppol BIS 3 as country-specific rules for the Netherlands.

This will not create a new document type. It will result in a new version of Peppol BIS 3.

This is a change in Peppol BIS 3 that is not backwards compatible; the additional rules are new for Peppol BIS 3. However, only suppliers in the Netherlands should be affected by these rules. Since these suppliers must already support NLCIUS, the technical impact of this change should be relatively small.

## 3 Approach

The NLCIUS defines three types of rules:

1. Compliancy rules for all invoices
2. Compliancy rules if the Supplier is in the Netherlands
3. Recommendations (also known as conformant rules)

Documents that do not conform to rules of types 1 and 2 are not considered valid NLCIUS documents, and result in errors when validating.

Documents that do not conform to rules of type 3 are considered valid NLCIUS documents, and only result in warnings when validating.

The approach taken in this document is as follows:

1. All current Peppol BIS 3 rules are taken as a given. Changing or removing Peppol BIS 3 rules is considered out of scope for this document.
2. NLCIUS Rules of type 1 are converted to rules of type 2, by adding ‘For suppliers in the Netherlands’ to the requirement.
3. Rules of type 2 are checked against existing Peppol BIS 3 rules:
    a. If there is a conflicting rule in Peppol BIS 3, the NLCIUS rule is dropped
    b. If there is no conflicting rule in Peppol BIS 3, the NLCIUS rule is added to the list

4. In line with OpenPeppol's statement that Peppol only works with compliant documents, all rules of type 3 are dropped.

5. The numbering of the Peppol BIS 3 rules is based on the order of fields in the BIS 3 Invoice/CreditNote document.

## 4 List of changes

| NLCIUS rule | Peppol BIS 3 rule | Modification | Explanation |
| ----------- | ----------------- | ------------ | ----------- |
| BR-NL-1 | NL-R-003 | PartyLegalEntity/CompanyID is not mandatory. Rule text is changed. | If company identifier is given, it should be KvK or OIN. BR-CO-26 requires that a company identifier, legal identifier, or VAT identifier is given. |
| BR-NL-2 | - | - | Already enforced by PEPPOL-EN16931-R003 |
| BR-NL-3 | NL-R-002 | - | Required by law for tax purposes |
| BR-NL-4 | NL-R-004 | - | Required by law for tax purposes |
| BR-NL-5 | NL-R-006 | - | Required by law for tax purposes |
| BR-NL-7 | - | - | Dropped due to conflict with PEPPOL-EN16931-P0100 and PEPPOL-EN16931-P0101 |
| BR-NL-8 | - | - | Dropped due to conflict with PEPPOL-EN16931-P0100 and PEPPOL-EN16931-P0101 |
| BR-NL-9 | NL-R-001 | Check CreditNoteTypeCode instead of InvoiceTypeCode. | BIS 3 does not support InvoiceTypeCode 384. Use 380 if no BillingReference is available. |
| BR-NL-10 | NL-R-005 | See BR-NL-1. | See BR-NL-1. |
| BR-NL-11 | NL-R-007 | | |
| BR-NL-12 | NL-R-008 | | |
| BR-NL-13 | NL-R-009 | Add ‘For suppliers in the Netherlands’ | |

All other rules (BR-NL-19 to BR-NL-35) are recommendations, and are dropped for Peppol BIS 3.

## 5 Detailed list Peppol BIS 3 NL validations

| NL-R-001    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, if the document is a creditnote, the document MUST contain an invoice reference (cac:BillingReference/cac:InvoiceDocumentReference/cbc:ID) |
| *Business rule (dutch)* | Indien de leverancier in Nederland is gevestigd (BT-40 = NL) en indien de Factuursoort (BT-3) de waarde 381 (Creditnota) heeft, moet het Vorige factuurnummer (BT-25) gevuld zijn. |
| *Context* | doc:CreditNote/cbc:CreditNoteTypeCode |
| *Test* | /*/cac:BillingReference/cac:InvoiceDocumentReference/cbc:ID |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-002    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands the supplier's address (cac:AccountingSupplierParty/cac:Party/cac:PostalAddress) MUST contain street name (cbc:StreetName), city (cbc:CityName) and postal zone (cbc:PostalZone) |
| *Business rule (dutch)* | Voor Nederlandse leveranciers (BT-40 = NL) moet het Adres leverancier (BG-5) de Adresregel 1 leverancier (BT-35), de Postcode leverancier (BT-38) en de Plaatsnaam leverancier (BT37) bevatten. |
| *Context* | doc:Invoice/cac:AccountingSupplierParty/cac:Party/cac:PostalAddress |
| *Test* | cbc:StreetName and cbc:CityName and cbc:PostalZone |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-003    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, the legal entity identifier MUST be either a KVK or OIN number (schemeID 0106 or 0190) |
| *Business rule (dutch)* | Voor Nederlandse leveranciers (BT-40 = NL) moet in het Registratienummer leverancier ofwel het KvK nummer worden vermeld (BT-30 met Scheme ID = 106), ofwel het OIN nummer (BT-30 met scheme ID = 190). |
| *Context* | doc:Invoice/cac:AccountingSupplierParty/cac:Party/cac:PartyLegalEntity/cbc:CompanyID |
| *Test* | (contains(concat(' ', string-join(cac:PartyLegalEntity/cbc:CompanyID/@schemeID, ' '), ' '), ' 0106 ') or contains(concat(' ', string-join(cac:PartyLegalEntity/cbc:CompanyID/@schemeID, ' '), ' '), ' 0190 ')) and (cac:PartyLegalEntity/cbc:CompanyID/normalize-space(.) != '') |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-004    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, if the customer is in the Netherlands, the customer address (cac:AccountingCustomerParty/cac:Party/cac:PostalAddress) MUST contain the street name (cbc:StreetName), the city (cbc:CityName) and the postal zone (cbc:PostalZone) |
| *Business rule (dutch)* | Indien de leverancier in Nederland is gevestigd (BT-40 = NL) moet voor Nederlandse afnemers (BT-55 = NL) het Adres afnemer(BG8) de Adresregel 1 afnemer (BT-50), de Postcode afnemer (BT-53) en de Plaatsnaam afnemer (BT-52) bevatten. |
| *Context* | doc:Invoice/cac:AccountingCustomerParty/cac:Party/cac:PostalAddress |
| *Test* | cac:Country/cbc:IdentificationCode != 'NL' or ( cbc:StreetName and cbc:CityName and cbc:PostalZone) |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-005    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, if the customer is in the Netherlands, the customer's legal entity identifier MUST be either a KVK or OIN number (schemeID 0106 or 0190) |
| *Business rule (dutch)* | Indien de leverancier in Nederland is gevestigd (BT-40 = NL) moet voor Nederlandse afnemers (BT-55 = NL) in het Registratienummer afnemer ofwel het KvK nummer worden vermeld (BT-47 met Scheme ID = 106), ofwel het OIN nummer (BT-47 met scheme ID = 190). |
| *Context* | doc:Invoice/cac:AccountingCustomerParty/cac:Party/cac:PartyLegalEntity/cbc:CompanyID |
| *Test* | (contains(concat(' ', string-join(cac:PartyLegalEntity/cbc:CompanyID/@schemeID, ' '), ' '), ' 0106 ') or contains(concat(' ', string-join(cac:PartyLegalEntity/cbc:CompanyID/@schemeID, ' '), ' '), ' 0190 ')) and (cac:PartyLegalEntity/cbc:CompanyID/normalize-space(.) != '') |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-006    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, if the fiscal representative is in the Netherlands, the representative's address (cac:TaxRepresentativeParty/cac:PostalAddress) MUST contain street name (cbc:StreetName), city (cbc:CityName) and postal zone (cbc:PostalZone) |
| *Business rule (dutch)* | Indien de leverancier in Nederland is gevestigd (BT-40 = NL) moet voor Nederlandse fiscaal vertegenwoordigers (BT-69=NL) het adres van de fiscaal vertegenwoordiger, als het opgegeven is, de Adresregel 1 Fiscaal vertegenwoordiger (BT-64), de Postcode fiscaal vertegenwoordiger (BT-67) en de Plaatsnaam fiscaal vertegenwoordiger (BT-66) bevatten. |
| *Context* | doc:Invoice/cac:TaxRepresentativeParty/cac:PostalAddress |
| *Test* | (cac:Country/cbc:IdentificationCode != 'NL') or (cbc:StreetName and cbc:CityName and cbc:PostalZone) |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-007    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, the supplier MUST provide a means of payment (cac:PaymentMeans) if the payment is from customer to supplier |
| *Business rule (dutch)* | Het is verplicht voor Nederlandse leveranciers (BT-40 = NL) om de Betaalinstructies (BG-16) te vullen indien de betaling van afnemer naar leverancier gaat. |
| *Context* | doc:Invoice/cac:LegalMonetaryTotal |
| *Test* | xs:decimal(cbc:PayableAmount) <= 0.0 or (//cac:PaymentMeans) |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-008    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, the payment means code (cac:PaymentMeans/cbc:PaymentMeansCode) MUST be one of 30, 48, 49, 57, 58 or 59 |
| *Business rule (dutch)* | De Code betaalwijze (BT-81) mag voor Nederlandse leveranciers (BT-40 = NL) de waarden 58, 59, 57, 49, 30 of 48 hebben. |
| *Context* | doc:Invoice/cac:PaymentMeans |
| *Test* | normalize-space(cbc:PaymentMeansCode) = '30' or normalize-space(cbc:PaymentMeansCode) = '48' or normalize-space(cbc:PaymentMeansCode) = '49' or normalize-space(cbc:PaymentMeansCode) = '57' or normalize-space(cbc:PaymentMeansCode) = '58' or normalize-space(cbc:PaymentMeansCode) = '59' |
| *Remarks* | There is no check on supplier country in the context or test yet |


| NL-R-009    |             |
|-------------|-------------|
| *Business rule (english)* | For suppliers in the Netherlands, if an order line reference (BT-132) is used, there must be an order reference on the document level (BT-13) |
| *Business rule (dutch)* | Voor Nederlandse leveranciers (BT-40 = NL), indien de Referentie orderregel (BT-132) is gevuld, moet Inkoopordernummer (BT-13) gevuld zijn. |
| *Context* | doc:Invoice/cac:InvoiceLine/cac:OrderLineReference/cbc:LineID |
| *Test* | exists(/*/cac:OrderReference/cbc:ID) |
| *Remarks* | There is no check on supplier country in the context or test yet |
