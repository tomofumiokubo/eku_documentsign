---
title: "General Purpose Extended Key Usage (EKU) for Document Signing X.509 Certificates"
abbrev: "EKU for Document Signing"
docname: draft-ito-documentsigning-eku-01
category: info

ipr: trust200902
area: Security
workgroup: Individual 
keyword: Internet-Draft

stand_alone: yes
smart_quotes: no
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: T. Ito
    name: Tadahiko Ito
    organization: SECOM CO., LTD.
    email: tadahiko.ito.public@gmail.com
 -
    ins: T. Okubo
    name: Tomofumi Okubo
    organization: DigiCert, Inc.
    email: tomofumi.okubo+ietf@gmail.com
 -
    ins: S. Turner
    name: Sean Turner
    organization: sn3rd
    email: sean@sn3rd.com

normative:
  X.680:
      title: "Information technology - Abstract Syntax Notation One (ASN.1): Specification of basic notation"
      date: November 2015
      author:
        org: ITU-T
      seriesinfo:
        ISO/IEC: 8824-1:2015
        
--- abstract

{{!RFC5280}} specifies several extended key usages for X.509 certificates. This document defines a general purpose document signing extended key usage for X.509 public key certificates which restricts the usage of the certificates for document signing. 

--- middle

# Introduction

{{!RFC5280}} specifies several extended key usages for X.509 certificates. In addition, 
several extended key usage had been added{{!RFC7299}} as public OID under the IANA repository.
While usage of any extended key usage is bad practice for publicly trusted certificates, 
there are no public and general extended key usage explicitly assigned for Document Signing certificates. 
The current practice is to use id-kp-emailProtection, id-kp-codeSigning or vendor defined Object ID 
for general document signing purposes.

In circumstances where code signing and S/MIME certificates are also widely used for document signing, 
the technical or policy changes that are made to code signing and S/MIME certificates may cause 
unexpected behaviors or have an adverse impact such as 
decreased cryptographic agility on the document signing ecosystem and vice versa.

There is no issue if the vendor defined OIDs are used in a PKI (or a trust program) governed by the vendor.
However, if the OID is used outside of the vendor governance, the usage can easily become out of control
(e.g. - When the end user encounters vendor defined OIDs, they might want to ask that vendor about use of the certificate, however, the vendor may not know about the particular use. - If the issuance of the cert is not under the control of the OID owner, there is no way for the OID owner to know what the impact will be if any change is made to the OID in question, and it would restrict vendor's choice of OID management. etc.).
<!--何が問題かを、上に書いた。-->

Therefore, it is not favorable to use a vendor defined EKU for signing a document that is not governed by the vendor.

This document defines a general Document Signing extended key usage.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 {{!RFC2119}} {{!RFC8174}}
when, and only when, they appear in all capitals, as shown here.


# Extended Key usage for DocumentSigning
This specification defines the KeyPurposeId id-kp-documentSigning.
Inclusion of this KeyPurposeId in a certificate indicates that the
use of any Subject names in the certificate is restricted to use by a document signing.

Term of "Document Sign" in this paper is digitaly signing human readable data or data that is intended to be human readable by means of services and software.

## Extended Key Usage Values for Document Signing {#ext}
{{RFC5280}} specifies the EKU X.509 certificate extension for use in the Internet.  The extension indicates one or more purposes for which the certified public key is valid.  The EKU extension can be used in conjunction with the key usage extension, which indicates how the public key in the certificate is used, in a more basic cryptographic way.

The EKU extension syntax is repeated here for convenience:

~~~
    ExtKeyUsageSyntax  ::=  SEQUENCE SIZE (1..MAX) OF KeyPurposeId
    KeyPurposeId  ::=  OBJECT IDENTIFIER
~~~

This specification defines the KeyPurposeId id-kp-documentSigning. Inclusion of this KeyPurposeId in a certificate indicates that the use of any Subject names in the certificate is restricted to use by a document signing service or a software (along with any usages allowed by other EKU values).

<!-- the use of any Subject names in the certificate is restricted to use by a document signing service or a software. これはそのままでいいと思う？-->

~~~
    id-kp  OBJECT IDENTIFIER  ::=
        { iso(1) identified-organization(3) dod(6) internet(1)
          security(5) mechanisms(5) pkix(7) 3 }
    id-kp-documentSigning  OBJECT IDENTIFIER  ::=  { id-kp XX }
~~~

# Implications for a Certification Authority
The procedures and practices employed by a certification authority MUST ensure that the correct values for the EKU extension are inserted in each certificate that is issued.
Unless certificates are governed by a vendor specific PKI (or trust program), certificates that indicate usage for document signing MAY include the id-kp-documentSigning EKU extension. This does not encompass the mandatory usage of the id-kp-documentSigning EKU in conjunction with the vendor specific EKU. However, this does not restrict the CA from including multiple EKUs related to document signing.

# Security Considerations 
The Use of id-kp-documentSigning EKU can prevents the usage of id-kp-emailProtection for none-email purposes and  id-kp-codeSigning for signing objects other than binary codes. An id-kp-documentSigning EKU value does not introduce any new security or privacy concerns.


# IANA Considerations

This document requests that IANA make two assignments. One for the id-kp-documentSigning object identifier (OID), as defined in {{ext}}, for the EKU from the "SMI Security for PKIX Extended Key Purpose" (1.3.6.1.5.5.7.3) registry.  Another for the id-mod-docsign-eku, as defined in Appendix A, for the ASN.1 module {{X.680}} from the in the "SMI Security for PKIX Module Identifier" (1.3.6.1.5.5.7.0) registry.  No further action is necessary by IANA.

--- back

# Acknowledgments
{:numbered="false"}

We would like to thank Russ Housley for verifying the ASN.1 module.

# Appendix A. ASN.1 Module
{:numbered="false"}

The following ASN.1 module provides the complete definition of the
Document Signing EKU.

~~~
DocSignEKU { iso(1) identified-organization(3) dod(6) internet(1)
   security(5) mechanisms(5) pkix(7) id-mod(0) id-mod-docsign-eku(TBD1) }

   DEFINITIONS EXPLICIT TAGS ::=

   BEGIN

   -- EXPORTS ALL --

   -- IMPORTS NOTHING --

   -- OID Arc --

   id-kp  OBJECT IDENTIFIER  ::= {
     iso(1) identified-organization(3) dod(6) internet(1)
     security(5) mechanisms(5) pkix(7) kp(3) }

   -- Document Signing Extended Key Usage --

   id-kp-documentSigning OBJECT IDENTIFIER ::= { id-kp TBD2 }

   END
~~~
