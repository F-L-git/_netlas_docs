---
title: Domain WHOIS
description: Master the Domain WHOIS search on Netlas.io to find registrant information, domain registration dates, and registrar details.
---


# Domain WHOIS Data Collection

This data collection contains parsed domain WHOIS data, which is designed to identify domain owner and registrar information such as contact information, registrar details, registration dates, and expiration dates.

## Domain Whois Search Examples

Use the `domain` field to perform a unique search that returns a single result.

* Search by full domain name:
  ```
  domain:netlas.io
  ```

* Search by brand name:
  ```
  name:netlas
  ```

* Brand name zone search:
  ```
  domain:netlas.* level:[2 TO 3]
  ```

## Information Fields Search Examples

* Domains pointing on the same name servers
  ```
  name_servers:*.ns.facebook.com
  ```

* .COM domains, that will be deleted in 30 days or less
  ```
  status:redemptionPeriod zone:com
  ```

* Domains registered in January 2022 with uncovered emails 
  ```
  created_date:[2022-01-01 TO 2022-01-31] registrant.email:(* AND NOT (\"http://\" OR \"https://\" OR *privacy* OR *GDPR* OR *whois* OR \"rdds service\"))
  ```


## Search by Contact Info

One of the most effective ways to find domains managed by an organization is to filter by contact information. Fields `abuse` and `registrant.email` are perfect for this.

* Search by email address
  ```
  registrant.email:domain@fb.com
  ```

* Search by email server
  ```
  registrant.email:*@gmail.com
  ```

* By organization name
  ```
  registrant.organization:\"Meta Platforms\"
  ```


## Suspicious and Malicious Domains Search

Attackers can register domains with similar spelling to mislead visitors. The examples below allow you to identify domains that may host phishing or other suspicious resources.

*Examples:*

* Domains look like netlas.io (fuzzy search)
  ```
  domain:netlas.io~1
  ```

* Level 2 domains with the word “microsoft” in the “io” zone:
  ```
  domain:*microsoft* AND zone:io AND level:2
  ```

* Domains associated with binance.com somehow 
  ```
  domain:*binance.com* AND a:* AND NOT domain:(*.binance.com OR binance.com) AND level:2
  ```

Fuzzy queries with `~`` operator can be used to search similar spelling domains or different forms of words. You can specify the distance equal to 1 or 2 after the fuzzy operator. The default distance equals 2.


<br>
