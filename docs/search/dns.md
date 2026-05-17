---
title: DNS Registry
description: Explore the guide to using DNS Registry search tool. Discover domains, resolve DNS records, and analyze connections.
---


# DNS Registry Data Collection

The Netlas domain name system registry, which we continuously expand and periodically update. Every known to Netlas domain stored here. For each domain, Netlas stores the corresponding IP addresses as well as other types of DNS records.

Most commonly used fields are:

* `domain` – the domain name itself;
* `zone` – top level domain, for example “com”, or “net”;
* `level` – a level of the domain.


## Subdomains Search

The most convenient way to build a subdomain search query is using a wildcard `*` within the domain field.


Other useful filters are:

* zone – a top-level domain, for example “com” or “net”
* level – a level of the domain

*Examples:*

* Paypal.com subdomains: 
  ```
  domain:*.paypal.com
  ```

* Level 3 subdomains ends with 'mail' for google.com: 
  ```
  domain:*mail.google.com AND level:3
  ```

* Possible VoIP services: 
  ```
  domain:voip.* AND level:3
  ```


## TLDs Enumeration

Important results can be obtained in the asset discovery process by searching for an organization's domain in different domain zones. It helps with finding international infrastructure. You can use regex within the domain filed or the zone filed to build such queries.

*Examples:*

* Paypal domains in every TLDs: 
  ```
  domain:paypal.* level:2
  ```

* The same using regex:
  ```
  domain:/paypal\.[a-z0-9-]*/
  ```

* Paypal domains and subdomains in every TLDs (regex): 
  ```
  domain:/(.*\.)?paypal\.[a-z0-9-]*/
  ```

More information on regex usage [available here](search_basics.md).



## Search by Type of Record

Netlas collects commonly used types of DNS records, which can be used as search parameters:

* `a` – A-records, IP addresses associated with this domain
* `cname` – CNAME-records, domain name aliases
* `mx` – MX-records, point to email services for a domain
* `ns` – NS-records, identify the DNS servers responsible (authoritative) for a domain zone
* `txt` – TXT-records, used to hold descriptive text.

*Examples:*

```
a:1.1.1.1
```

```
a:"163.114.132.0/24"
```

```
ns:*.gov
```

```
ns:*.cloudflare.com
```

```
mx:*.zoho.com
```

```
zone:cn mx:*.google.com
```

```
!txt:"v=spf1" mx:* zone:gov level:2
```

<br>