---
title: IP WHOIS
description: Utilize Netlas.io to conduct IP WHOIS searches. Learn how to find owner information, geolocation data, and network range insights for any IP address.
---


# IP WHOIS Data Collection

This datacollection contains parsed IP WHOIS data that is designed to determine IP address ownership and control information, including IP address range, network provider name, contact information, and sometimes technical issues and abuse of contact information. This data is used for Responses collection enrichment.

## Search by IP Address

Use the `ip` field to perform a unique search that returns a single result.

* Exact match: 
  ```
  ip:8.8.8.8
  ```
* Multiple addresses: 
  ```
  ip:(1.0.0.1 OR 1.1.1.1)
  ```
* IP range: 
  ```
  ip:[195.6.151.68 TO 195.6.151.70]
  ```

## Networks and Autonomus Systems

Netlas.io whois search tool is useful in identifying the organization's IT infrastructure exposed on the internet. Especially when it comes to large organizations. These organizations usually use a large number of IP addresses. Administrators of such IT infrastructures register managed IP ranges as networks and autonomous systems. 

The whois protocol is not strictly standardized. Whois records sometimes are inaccurate, incomplete, or irrelevant. Company name can be specified as "Organization" or as "Description" for example. The structure of the Netlas whois document is very close to the original whois records. So our whois collection also does not solve these shortcomings. Keep this in mind when you build search queries.

The most used fileds are:

* `net.name` – symbolic identificator of a network.
* `net.organization` – name of the organization managing the network.
* `net.description` – some additional info, oftely contains organization name, department or location.

In most cases, whois servers return additional information about the larger network (in which the requested network is included) and related autonomous system. This information is also included in each document within fields related_nets and asn. The related_nets field has the same subfields as the net field. The asn field has no asn.organization and asn.description subfields.

*Examples:*

* Search by organization name:  
  ```
  net.organization:Mandiant
  ```
* Search by AS name: 
  ```
  asn.name:CERN
  ```
* Search by any name (net, related_nets or ASN): 
  ```
  \*.name:*FACEBOOK*
  ```
* Search by descriotion: 
  ```
  net.description:"DDoS mitigation"
  ```
* Search "Microsoft" anywhere in raw whois response: 
  ```
  raw:microsoft
  ```

## Filter by Contacts and Geolocation

Whois records often contain contacts of administrators for resolving technical issues. You can use those contacts as a search filter. It allows you to trace the links between networks and domain names, peoples and networks, and even the geolocation of networks.

*Examples:*

* Networks managed by Google: 
  ```
  abuse:"network-abuse@google.com" OR net.contacts.emails:"network-abuse@google.com"
  ```
* Educational networks: 
  ```
  abuse:/.*\.edu(\.[a-zA-Z0-9]*)?/
  ```
* Contacts with Tokyo phones: 
  ```
  net.contacts.phones:/+81[- ]?03([- ]?[0-9]){8}/
  ```
* Hong Kong networks: 
  ```
  net.сountry:"HK"
  ```
* United Arab Emirates networks: 
  ```
  net.country:AE
  ```


<br>

