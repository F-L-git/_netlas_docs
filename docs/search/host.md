---
title: IP/Domain Info
description: Details about any host with IP/Domain Info tool. Uncover associated data, track ownership records, and assess cybersecurity risks effectively.
---


# IP/Domain Info

:netlas-host: __IP/Domain Info__ tool provides an easy way to obtain a summary of Netlas data for a specific IP address or domain name. By aggregating data from nearly all Netlas data collections in a single request, users receive a thorough overview of the target.


## Usage

Input a valid IP address or domain name to retrieve data.

Without an argument, the tool returns a summary for the requester's IP, allowing you to quickly assess your external IP by visiting [Netlas app](https://app.netlas.io/host/).


The IP/Domain Info tool is limited to single-host queries by IP address or domain name. For multi-target investigations, query each target individually. See the [automation](../automation/search_tools_api.md) section to get ideas on how to batch the operations.

!!! warning "The IP/Domain info tool doesn't support complex queries, conditions, and operators."


## Contents

The tool returns different data for IPs and domains. The most majority of fields are optional. 

<center>
![IP/Domain Info tool](img/host_example-l.png#only-light){ loading=lazy }
![IP/Domain Info tool](img/host_example-d.png#only-dark){ loading=lazy }
</center>

??? info "Please note, data availability depends on your pricing plan"

    For example, if your pricing plan does not provide you with access to contact details such as phone numbers and email addresses, this data will not be returned (displayed) by any of Netlas tools.

### Anonymity Labels

Displayed next to the IP address are labels indicating if the IP is associated with a TOR exit node, a VPN, or a proxy service.

<center>
![TOR / VPN / Proxy bages](img/anonimity_bages-l.png#only-light){ .bordered loading=lazy }
![TOR / VPN / Proxy bages](img/anonimity_bages-d.png#only-dark){ .bordered loading=lazy }
</center>


- __TOR__ label displayed if the IP address hosts a TOR exit node according to [Onionoo protocol](https://metrics.torproject.org/onionoo.html) data. Updated daily. 

- __VPN__ label displayed if the scanner has detected a software of the corresponding category. Updated during scanning.

- __Proxy__ label displayed if the scanner has detected socks-proxy service. Updated during scanning.



### IP-to-Organization

Identify the organization managing an IP address using `Organization` and `PTR` fields.

The `Organization` field in the IP info view is a calculated property:

1. By default it equals to `net.organization` field.
2. If `net.organization` is  undefined, it equals to `net.description`.
3. If `net.description` is also undefined, it  equals to `net.name`.

The `PTR`, if present, typically indicates a domain owned by the organization.


### Threat Intelligence Data

For an IP address or domain, __threat intelligence__ records can also be displayed. This information is provided by our partners.

Netlas stores and displays IoCs (Indicators of Compromise) for the past year, so please take note of the date in the first column. Some IoCs may be reported as false positives; these will be marked with a special symbol in the last column. The IoCs data is updated daily.

Threat intelligence data is available only in the IP/Domain Info Tool.

<center>
![Indicators of Compromise in Netlas](img/host_iocs-l.png#only-light){ .bordered loading=lazy }
![Indicators of Compromise in Netlas](img/host_iocs-d.png#only-dark){ .bordered loading=lazy }
</center>




### Scan Results

Display of __scan results__ varies between IP addresses and domains:

- For IP addresses, all available protocols are displayed, including HTTP requested by IP.

- For domains, only the HTTP protocol scan results are displayed.


=== "IP address view"

    ![Scan results on IP address view](img/host_responses_ip-l.png#only-light){ .bordered loading=lazy }
    ![Scan results on IP address view](img/host_responses_ip-d.png#only-dark){ .bordered loading=lazy }

=== "Domain view"

    ![Scan results on Domain view](img/host_responses_domain-l.png#only-light){ .bordered loading=lazy }
    ![Scan results on Domain view](img/host_responses_domain-d.png#only-dark){ .bordered loading=lazy }

For comprehensive scan data, use the __Responses Search__ button.

<!-- 
## Automation

Access to the IP/Domain Info tool is provided via API, SDK, and CLI. Learn how to setup Netlas SDK and CLI tools from the [automation section](../automation/index.md).

??? abstract "Example JSON documents with IP/domain information"

    === "135.125.237.168"
        ``` json linenums="1"
        {
          "ip": "135.125.237.168",
          "type": "ip",  
          "privacy": {
            "is_vpn": false,
            "is_proxy": false,
            "is_tor": false
          },
          "organization": "OVH GmbH",
          "geo": {
            "continent": "Europe",
            "country": "France",
            "location": {
              "latitude": 48.8582,
              "accuracy_radius": 500,
              "time_zone": "Europe/Paris",
              "longitude": 2.3387
            },
            "registered_country": "France"
          },
          "domains_count": 3,
          "domains": [
            "mail.netlas.io",
            "app.netlas.io",
            "pay.netlas.io"
          ],
          "software": [
            {
              "tag": [
                {
                  "debian": {
                    "version": ""
                  },
                  "name": "debian",
                  "cpe": [
                    "cpe:/o:debian:debian_linux"
                  ],
                  "description": "Debian is a Linux software which is a free open-source software.",
                  "fullname": "Debian",
                  "category": [
                    "Operating systems"
                  ]
                },
                {
                  "name": "dovecot",
                  "cpe": [
                    "cpe:/a:dovecot:dovecot"
                  ],
                  "dovecot": {
                    "version": ""
                  },
                  "description": "Dovecot is an open-source IMAP and POP3 server for Unix-like operating systems, written primarily with security in mind",
                  "fullname": "Dovecot",
                  "category": [
                    "Mail server"
                  ]
                }
              ],
              "uri": "imaps://135.125.237.168:993"
            },
            {
              "tag": [
                {
                  "nginx": {
                    "version": ""
                  },
                  "name": "nginx",
                  "cpe": [
                    "cpe:/a:nginx:nginx"
                  ],
                  "description": "Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.",
                  "fullname": "Nginx",
                  "category": [
                    "Web servers",
                    "Reverse proxies"
                  ]
                }
              ],
              "uri": "https://135.125.237.168:443/"
            },
            {
              "tag": [
                {
                  "debian": {
                    "version": ""
                  },
                  "name": "debian",
                  "cpe": [
                    "cpe:/o:debian:debian_linux"
                  ],
                  "description": "Debian is a Linux software which is a free open-source software.",
                  "fullname": "Debian",
                  "category": [
                    "Operating systems"
                  ]
                },
                {
                  "name": "dovecot",
                  "cpe": [
                    "cpe:/a:dovecot:dovecot"
                  ],
                  "dovecot": {
                    "version": ""
                  },
                  "description": "Dovecot is an open-source IMAP and POP3 server for Unix-like operating systems, written primarily with security in mind",
                  "fullname": "Dovecot",
                  "category": [
                    "Mail server"
                  ]
                }
              ],
              "uri": "imapstarttls://135.125.237.168:143"
            }
          ],
          "ptr": [
            "mail.netlas.io"
          ],
          "whois": {
            "abuse": "abuse@ovh.net",
            "ip": {
              "gte": "135.125.236.0",
              "lte": "135.125.237.255"
            },
            "related_nets": [
              {
                "created": "2020-11-03T13:34:30Z",
                "start_ip": "135.125.128.0",
                "range": "135.125.128.0 - 135.125.255.255",
                "cidr": [
                  "135.125.128.0/17"
                ],
                "net_size": 32767,
                "updated": "2020-11-03T13:34:30Z",
                "end_ip": "135.125.255.255"
              }
            ],
            "net": {
              "country": "DE",
              "address": "St. Johanner Str. 41-43\n66111 Saarbrucken\nDeutschland",
              "created": "2021-03-30T07:33:51Z",
              "range": "135.125.236.0 - 135.125.237.255",
              "handle": "OTC13-RIPE",
              "organization": "OVH GmbH",
              "name": "VPS-DE2",
              "start_ip": "135.125.236.0",
              "cidr": [
                "135.125.236.0/23"
              ],
              "net_size": 511,
              "updated": "2021-03-30T07:33:51Z",
              "end_ip": "135.125.237.255",
              "contacts": {
                "emails": [
                  "abuse@ovh.net"
                ]
              }
            },
            "asn": {
              "number": [
                "16276"
              ],
              "registry": "ripencc",
              "country": "FR",
              "name": "OVH",
              "cidr": "135.125.128.0/17",
              "updated": "2000-10-11"
            }
          },
          "ports": [
            {
              "prot4": "tcp",
              "protocol": "imaps",
              "port": 993,
              "prot7": "imap"
            },
            {
              "prot4": "tcp",
              "protocol": "smtp",
              "port": 25,
              "prot7": "smtp"
            },
            {
              "prot4": "tcp",
              "protocol": "https",
              "port": 443,
              "prot7": "http"
            },
            {
              "prot4": "tcp",
              "protocol": "imapstarttls",
              "port": 143,
              "prot7": "imap"
            },
            {
              "prot4": "tcp",
              "protocol": "smtps",
              "port": 465,
              "prot7": "smtp"
            },
            {
              "prot4": "tcp",
              "protocol": "smtpstarttls",
              "port": 25,
              "prot7": "smtp"
            }
          ]
        }
        ```

    === "app.netlas.io"

        ``` json linenums="1"
        {
          "domain": "app.netlas.io",
          "type": "domain",
          "related_domains_count": 4,
          "related_domains": [
            "netlas.io",
            "pay.netlas.io",
            "www.netlas.io",
            "mail.netlas.io"
          ],
          "dns": {
            "a": [
              "135.125.237.168"
            ],
            "zone": "io",
            "level": 3
          },
          "whois": {
            "server": "whois.namecheap.com",
            "extension": "io",
            "last_updated": "2023-10-25T10:53:05.536Z",
            "registrar": {
              "phone": "+1.9854014545",
              "referral_url": "http://www.namecheap.com",
              "name": "NAMECHEAP INC",
              "id": "1068",
              "email": "abuse@namecheap.com"
            },
            "technical": {
              "country": "IS",
              "province": "Capital Region",
              "city": "Reykjavik",
              "phone": "+354.4212434",
              "street": "Kalkofnsvegur 2",
              "organization": "Privacy service provided by Withheld for Privacy ehf",
              "name": "Redacted for Privacy",
              "postal_code": "101",
              "email": "07da1849e7814858a31ea085d9e8c099.protect@withheldforprivacy.com"
            },
            "whois_server": "whois.namecheap.com",
            "level": 2,
            "name_servers": [
              "pdns1.registrar-servers.com",
              "pdns2.registrar-servers.com"
            ],
            "expiration_date": "2026-11-06T08:10:14.820Z",
            "punycode": "netlas.io",
            "zone": "io",
            "stats": {
              "retries": 0,
              "quota_retries": 0,
              "parser": "no_error",
              "was_queued": false,
              "total_time": 5661529222,
              "error": "no_error"
            },
            "administrative": {
              "country": "IS",
              "province": "Capital Region",
              "city": "Reykjavik",
              "phone": "+354.4212434",
              "street": "Kalkofnsvegur 2",
              "organization": "Privacy service provided by Withheld for Privacy ehf",
              "name": "Redacted for Privacy",
              "postal_code": "101",
              "email": "07da1849e7814858a31ea085d9e8c099.protect@withheldforprivacy.com"
            },
            "domain": "netlas.io",
            "name": "netlas",
            "id": "1f378468bddb44f3a084571e1e028e55-DONUTS",
            "created_date": "2020-11-06T08:10:14.820Z",
            "registrant": {
              "country": "IS",
              "province": "Capital Region",
              "city": "Reykjavik",
              "phone": "+354.4212434",
              "street": "Kalkofnsvegur 2",
              "organization": "Privacy service provided by Withheld for Privacy ehf",
              "name": "Redacted for Privacy",
              "postal_code": "101",
              "email": "07da1849e7814858a31ea085d9e8c099.protect@withheldforprivacy.com"
            },
            "updated_date": "2022-09-28T09:38:39.210Z",
            "extracted_domain": "netlas.io",
            "status": [
              "clienttransferprohibited"
            ]
          },
          "software": [
            {
              "tag": [
                {
                  "django": {
                    "version": ""
                  },
                  "name": "django",
                  "cpe": [
                    "cpe:/a:djangoproject:django"
                  ],
                  "description": "Django is a Python-based free and open-source web application framework.",
                  "fullname": "Django",
                  "category": [
                    "Web frameworks"
                  ]
                },
                {
                  "google_font_api": {
                    "version": ""
                  },
                  "name": "google_font_api",
                  "description": "Google Font API is a web service that supports open-source font files that can be used on your web designs.",
                  "fullname": "Google Font API",
                  "category": [
                    "Font scripts"
                  ]
                },
                {
                  "nginx": {
                    "version": ""
                  },
                  "name": "nginx",
                  "cpe": [
                    "cpe:/a:nginx:nginx"
                  ],
                  "description": "Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.",
                  "fullname": "Nginx",
                  "category": [
                    "Web servers",
                    "Reverse proxies"
                  ]
                }
              ],
              "uri": "https://app.netlas.io:443/"
            }
          ],
          "ports": [
            {
              "prot4": "tcp",
              "protocol": "https",
              "port": 443,
              "prot7": "http"
            }
          ],
          "related_domains_query": "(domain:*.netlas.io AND NOT domain:app.netlas.io) OR domain:netlas.io"
        }
        ```

    
    !!! warning "Please, keep in  mind:"

        1. __Not all possible fields are shown.__    
        For example, Netlas scanners were unable to determine software versions, so there is no data on possible vulnerabilities.

        2. __The document structure may change over time.__    
        We try to avoid this, but in rare cases we have to change the structure of documents. This may be due to the addition of new data, sometimes due to optimization. Check the release notes for updates.


### CLI Commands

When using the Netlas CLI, the IP/Domain tool is accessible through the `host` command. Use `--help` for instructions.

``` sh
netlas host --help
```

Query examples:

``` sh
netlas host "135.125.237.168"
```

``` sh
netlas host "app.netlas.io"
```

Use the `host` command without arguments to query data related to your external IP address:

``` sh
netlas host
```

To reduce the amount of output data use `-i` (`--include`) to specify a list of fields to include in response or `-e` (`--exclude`) to filter out any fields. Both of these options cannot be used in the same request.

``` sh
netlas host -i "geo.location.time_zone,ip"
```

You can switch between YAML (used by default) and JSON output with the `-f` option:

``` sh
netlas host "google.com" -f json -i "whois.registrant.organization"
```

The response will be: 
``` json
{"whois": {"registrant": {"organization": "Google LLC"}}}
```

We recommend using the Netlas CLI in conjunction with the `jq` utility ([see the website](https://jqlang.github.io/jq/)). It is a lightweight and flexible command-line JSON processor. It allows you to perform a variety of manipulations with the output. For example, you can access a value by key:

``` sh
netlas host "google.com" -f json | jq '.whois.registrant.organization'
```

If you execute the command, you will get `"Google LLC"`. Use the `jq -r` to remove quotes:

``` sh
netlas host "8.8.8.8" -f json | jq -r '.ptr[0]'
```

The result should be `dns.google`.

### SDK Methods

The `Netlas.host` method in the Netlas package allows for easy fetching of host-related information.

::: netlas.client.Netlas.host
    handler: python
    options:
      docstring_style: sphinx
      heading_level: none
      show_root_heading: true
      show_root_toc_entry: false
      show_source: false


Here is how you can implement the last two examples from the "CLI commands" section above:

``` py
import netlas

api_key = "YOUR_API_KEY"
netlas_connection = netlas.Netlas(api_key=api_key)

# Domain query example
domain_info = netlas_connection.host(host="google.com")
print(domain_info['whois']['registrant']['organization'])

# IP query example
ip_info = netlas_connection.host(host="8.8.8.8")
print(ip_info['ptr'][0])
```


### API Endpoints

You can query IP/Domain Info tool using API endpoint `/host/{ip_or_domain}/`. Here are two queries to the `/host/` endpoint made with the command line utill `curl`:

``` sh
~ % curl -X 'GET' \
  'https://app.netlas.io/api/host/google.com/?fields=whois.registrant.organization&source_type=include' \
  -H 'accept: application/json' \
  -H 'X-API-Key: your_API-key_here'
```

The response will be:

``` json
{"whois":{"registrant":{"organization":"Google LLC"}}}
```

Netlas API methods return JSON objects, so you can use the `jq` utility to format and filter the output:

``` sh
~ % curl -X 'GET' -s \
  'https://app.netlas.io/api/host/8.8.8.8/?fields=&source_type=include' \
  -H 'accept: application/json' \
  -H 'X-API-Key: your_API-key_here' \
| jq -r '.ptr[0]' 
```

The output should be `dns.google`.

Refer to the [automation section](../automation/index.md) to get more insights on how to use Netlas API and SDK.
 -->
<br>

