---
title: SSL Certificates
description: Learn how to use SSL Certificates search. Find certificate details, expiry information, and associated domains with this guide.
---


# SSL Certificates Data Collection

This collection contains certificates collected from the Certificate Transparency Log and the certificates obtained with host responses during scans. The collection does not contain duplicates.

Most commonly used fields are:

* `certificate.names` – domain names and wildcards;
* `certificate.issuer_dn` - Distinguished Name;
* `certificate.subject_dn` - User Certificate information;
* `certificate.fingerprint_md5` or `fingerprint_sha1` or `fingerprint_sha256`;
* `certificate.validity.start` - start of certificate validity;
* `certificate.validity.end` - end of certificate validity.

## Organizations & Certification Authorities

The `certificate.issuer_dn` field represents the certificate issuer's distinguished name. The `certificate.subject_dn` field is the analog for the organization for which the certificate is issued. More information about the issuer and the subject of the certificate is available through subfields c`ertificate.issuer` and `certificate.subject`. Check out the mapping panel on the right side of the search screen to familiarise yourself with available search fields.

*Examples:*

```
certificate.subject.organization:"Microsoft Corporation"
```

```
certificate.issuer_dn:"Let's Encrypt"
```

```
certificate.subject_dn:"netlas.io"
```

```
certificate.issuer.country:CN
```

```
certificate.subject.surname:*
```

Please note that all subfields of the `certificate.subject` filter are optional. The `common_name` subfield is mandatory in certificates used for SSL/TLS connections. 

There are three types of certificates depending on the number of validation steps during the certificate issuance procedure:

* *Domain Validated (DV)* certificates provide the lowest level of authentication and poor certificate content.
* *Organization Validated (OV)* certificates provide additional checks during the issuance procedure, these certificates contain more information about the subject, e.g. name of the organization.
* *Extended Validation (EV)* certificates are the most trusted ones and contain maximum information about the subject.

Compare the contents of the subject_dn field for certificates filtered by:

```
certificate.validation_level:EV
```

```
certificate.validation_level:OV
```

```
certificate.validation_level:DV
```

## Certificate Validity Filters

* Certificates valid from the beginning of 2022:
  ```
  certificate.validity.start:>2022
  ```

* Certs expires in May, 2022:
  ```
  certificate.validity.end:[2022-05-01 TO 2022-05-31]
  ```

* Apple certs valid in May, 2022:
  ```
  certificate.subject.organization:"Apple Inc." certificate.validity.start:<2022-05 certificate.validity.end:>2022-06
  ```

## Certificate Purpose Filters

Netlas collects not only certificates used for SSL/TLS connection establishing. Сertificate Transparency Log, which Netlas uses as the main source of certificates, contains code signing certificates, CA certificates and others. You can query different types of certificates using subfields of the `certificate.extensions.key_usage` and `certificate.extensions.extended_key_usage`.

*Examples:*

* Code signing certificates issued by Apple: 
  ```
  certificate.extensions.extended_key_usage.code_signing:* certificate.issuer.organization:Apple
  ```

* Government certificates used for encrypting user data: 
  ```
  certificate.extensions.key_usage.data_encipherment:true certificate.issuer.organization:Government
  ```

* RSA Security signing certificates: 
  ```
  certificate.extensions.key_usage.certificate_sign:true certificate.issuer.organization:"RSA Security"
  ```

* Certificates used for various Microsoft technologies issued by Microsoft: 
  ```
  certificate.extensions.extended_key_usage.microsoft_\*:true certificate.issuer_dn:Microsoft
  ```

* Time-stamp service certifiacates: 
  ```
  certificate.extensions.extended_key_usage.time_stamping:true certificate.subject_dn:"*time stamp*"
  ```

## Inappropriate Field Types Bug

!!! warning "Unexpected search behavior"

    We made a mistake when creating the database schema for storing certificates. An inappropriate field type was used. This issue is not critical but reduces usability. We are sorry about that. It's even more annoying that we can't fix this right now. Certificate collection contains billions of certs. We have to reindex this entire volume to correct this bug. We will definitely fix this when we finish developing the main features.


This mistake causes an issue when you search using domain names within the certificate.subject_dn and certifiacate.issuer_dn fields or subfields of certificate.subject and certificate.issuer. It's impossible to search by exact match. Any search query you build executed like `*query*`. It means, when you search `certificate.subject.common_name:microsoft.com` you get not only an exact match of `"microsoft.com"`, but `"*.microsoft.com"`, and something like `"onedrive-microsoft.com"`, and even `"mail.cert-microsoft.com.cp-34.webhostbox.net"`.


*Query examples affected by this bug:*

```
certificate.subject.common_name:"t.me"
```

```
certificate.subject_dn:"kraken.com"
```

```
certificate.subject_dn:"mail.go"
```

<br>