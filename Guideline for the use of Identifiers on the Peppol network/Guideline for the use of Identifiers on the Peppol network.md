# Guideline for the use of identifier schemes for recipients on the Peppol Network

*Jelte Jansen, Patrick van Heijst, Wim Kok*

Version 1.0, 2021-03-09

# 1 Introduction

Document exchange on the Peppol network is performed using a 4-corner 
model: A trading entity (corner 1) uses an access point (corner 2) to send 
a business document to the receiving access point (corner 3) of a receiving 
trading entity (corner 4).

The sending access point locates the receiving access point by a published 
identifier of the receiving trading entity. This identifier can be one of 
many different identifier schemes. Examples are a country’s Chamber of 
Commerce number, a VAT number, a GLN, et cetera. The code list for PEPPOL 
BIS documents can be found [here](https://docs.peppol.eu/poacc/billing/3.0/codelist/eas/).

The specific scheme that is used can be a de-facto standard within a sector 
or country, or it can be bilaterally agreed between the sender and the 
recipient.

In the Netherlands, the Peppol Authority Specific Requirements (PASR) 
require that for a Dutch organization, at least the OIN, KvK number, or VAT 
is published. Identifiers from other schemes may be published as well, as 
long as at least an OIN, KvK, or VAT-number is published.

But even aside from this requirement, in order to improve automation and 
automatic discovery, the amount of data that needs to be bilaterally 
communicated should be kept as small as possible.

Therefore, it’s important that the sending organization knows which 
identifier scheme(s) to look for on the Peppol network; suppose that a 
receiving trading entity only publishes it’s VAT number, but the sending 
trading entity only looks for GLN, the latter might erroneously conclude 
that the recipient is not present on the network. This document provides 
recommendations for Dutch organizations on the Peppol network regarding:

* the identifiers to use when publishing receiving capabilities (section 
  3);
* which identifiers to use when looking up a potential recipient on the 
  network (section 4);
* the recommended method to derive the routing information from the 
  document context, if it isn't provided by external means (section 5);
* an explanation of which OIN identifier scheme(s) to use (section 6);
* a section on the case where an organization has multiple service
  providers or endpoints


# 2. Overview of common identifier schemes in the Netherlands

The scheme codes which are used to specify the identifier scheme are a 
combination of the ISO6523 ICD list, and a Peppol-specific extension list 
(codes starting with 9).

The common identifiers in the Netherlands are:

* KvK (Chamber of Commerce) number, code 0106
* OIN (Governmental organization number), codes 0190 and 9954
* (Dutch) VAT number, code 9944

Within specific sectors, GLN numbers (code 0088) may also be quite common, 
but they see very little, if any, use outside of those sectors.


# 3. Identifier scheme(s) to use to publish receiving capabilities

When publishing a recipient on the Peppol network, the recommendation is to 
publish their legal registration identifier as well as their VAT number. 

The legal registration identifier for companies in the Netherlands is their 
KvK number, with scheme identifier 0106.

The legal registration identifier for governmental organizations is their 
OIN number, with scheme identifier 0190 and 9954, depending on document 
type version.

The VAT number for organizations in the Netherlands is NL:VAT, with scheme 
identifier 9944.

One of these identifiers must be published, as per the PASR for the Netherlands:

> Service providers MUST register end users with at least scheme ID NL:KVK 
> (0106) or NL:OIN (0190) or NL:VAT (9944). Other identifiers can be 
> registered simultaneously. In case an end user does not have an NL:KVK, 
> NL:OIN or NL:VAT identifier and cannot obtain one of these identifiers, 
> the end user can be registered with other available scheme ID’s like IBAN
> or GLN.

Aside from the requirement itself, a second reason to publish the legal 
registration identifier is that these are well-known within the 
Netherlands, and are easy to find in publicly available central databases 
which can be searched by organization name. The reason to publish the VAT 
number is that these are often used by default in an international context. 
Publishing both will increase the chance of successful (automatic) 
discovery of the recipient on the network.

A summarized table of these guidelines:

Identifier | SchemeID | How to use
--|--|--
KVK | 0106 | Used as primary ID for organizations without OIN
OIN | 0190 & 9954 | Used as primary ID for organizations with OIN
VAT |  9944 | Used for any VAT-registered organization
IBAN | 9918 | Used as fallback for organizations without VAT

In the publication of these identifiers should be kept synchronized, and 
should not point to different endpoints or contain different metadata 
properties, so that documents sent to a VAT number are not routed or 
processed differently than documents sent to a KvK number. The only 
exception for this is the difference in document types for the different 
OIN schemes 0190 and 9954, see section 6.

Organizations can choose to publish additional identifiers that are 
available to them. It is strongly recommended that the publication of those 
identifiers are then kept synchronized as well. While publishing multiple 
identifiers does further increase discoverability, it comes with an 
additional maintenance burden, and a higher chance of erroneous data being 
published.

# 4. Identifier scheme(s) to use when looking up receiving trading entities

Since most organizations in the Netherlands have either a KvK- or 
OIN-number, these identifiers should be the default. However, in a small 
number of cases, an organization might not have a KvK- or OIN-number.

Therefore, to maximize the chance that a recipient is found on the network, 
it is recommended that a sender first checks whether the KvK- or OIN-number 
of the recipient exists on the network. If it cannot be found, it is 
recommended that the (Dutch) VAT number is checked, followed by any other 
identifiers that are known to the sending party.

# 5. Deriving the recipient’s identifier from Peppol BIS documents

When constructing the message envelope containing routing information for 
the message, the recipient identifier can be provided by some external 
context, such as a user setting it directly. However, in some use-cases the 
envelope is derived from the contents of the document itself, without 
additional information. This section describes the recommended approach to 
find the recipient identifier in the Peppol BIS document.

Per specification, the field to provide the identifier to use in routing is 
cbc:EndpointID. This field must contain the exact identifier that can be 
used to locate the access point of the recipient. It is recommended that 
the approach in section 4 is used to set its value.

For document types where EndpointID is not mandatory (such as SI-UBL) and 
not set, there are a number of fields that might contain the recipient 
identifier, and it is recommended they are treated as follows, in order of 
priority.

1. `cac:PartyLegalEntity/cbc:CompanyID`: While this field may only contain 
   official ISO ICD scheme values, it is the field where the KvK- and 
   OIN-number is used.
2. `cac:PartyIdentification/cbc:ID`: This field is optional, but it may 
   contain a scheme and identifier as well. This field can contain the full 
   Peppol list of identifier schemes, including VAT and GLN.
3. `cac:PartyTaxScheme/cbc:CompanyID`: If the VAT number starts with NL, the
   Dutch VAT scheme (9944) could be used with this value. This is only 
   recommended if none of the earlier identifiers were sufficient.

# 6. Identifier schemes for OIN numbers

There are two identifier schemes in use for governmental organizations: 
0190 and 9954.

The reason for this is historic: at the time of publication of SI-UBL 1.2, 
the 0190 code was not registered on the ISO 6523 ICD list yet. Peppol 
maintains it own extended identifier scheme list (marked by the number 
starting with 99), and the OIN scheme identifier in that list was 9954.

When SI-UBL 2.0 and Peppol BIS 3 were published, 0190 had been registered, 
and could be used with these documents.

Therefore, the identifier scheme to use for OIN is:

* 0190 with Peppol BIS 3 and SI-UBL 2.0 documents (and later)
* 9954 with SI-UBL 1.2 documents

Since all three of these document types are currently in use, OIN 
recipients must be published with both 0190 (for Peppol BIS 3 and SI-UBL 
2.0) and 9954 (for SI-UBL 1.2 documents) identifier schemes. When looking 
up recipients, whether to use 0190 or 9954 must be based on the document 
type.

If and when SI-UBL 1.2 is phased out in the future, only the 0190 scheme 
identifier will be in use.

# 7. Multiple service providers and endpoints

There are organizations that intentionally publish multiple identifiers 
with different metadata properties. One such scenario is that incoming 
documents can then internally be routed based on the identifier it was sent 
to, for instance by publishing different GLN or IBAN numbers for different 
departments. Another scenario is large organizations that have mutiple 
entry points in the Peppol network, such as when different departments or 
branches have different Peppol Service Providers.

While the practice of publishing mutiple identifiers for different 
processes is not prohibited, a central identifier for the entire 
organization must still be published to comply with the PASR.

Before the PASR requirement, the recommendation for such cases was to not 
publish a central identifier, but to use more specific identifiers (such as 
GLN) and bilaterally exchange those identifiers with suppliers. Any 
documents would be delivered to the right endpoints, but this approach 
would also stop other organizations from automatically finding them on the 
network. That scenario prevents a Peppol-first approach, or automatic 
discovery in general.

On the other hand, publishing a central identifier when multiple endpoints 
are in use, as is now required, can also introduce problems. Senders that 
use automatic discovery will send their document to the endpoint of the 
central identifier, and not the endpoint of the more specific identifier.

This means that organizations that can be reached on different endpoints 
should have an internal method to reroute documents in the organization. 
The recommended field to base such internal routing on is 
`cac:PartyIdentification/cbc:ID`.

How such internal routing is implemented is beyond the scope of this 
guideline, as this is entirely dependent on the architecture of the local 
systems.
