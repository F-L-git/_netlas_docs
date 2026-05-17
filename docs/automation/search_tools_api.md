---
title: Search Tools API
description: Details the Host API for single IP/domain queries and the Search Query API for complex, multi-parameter searches, along with use cases and practical examples.
---

# Search Tools API

The Netlas [Search Tools](../search/index.md) API allows you to perform search queries across any Netlas data collection from third-party applications and scripts.

Before you start building your integration, you need to decide which data you want to retrieve. It's important to distinguish between retrieving data for individual hosts and data for search queries.

- **Host API**: This specific endpoint in the Netlas API is designed to retrieve detailed information about a single IP address or domain. It is ideal for targeted queries where aggregated data about a particular host is required.
- **Search Query API**: This set of endpoints within the Netlas API enables complex and diverse queries across different parameters and data indices. These endpoints are suited for broader searches, utilizing [Search Query Language](../search/search_basics.md).

| Host API | Search Query API |
| -------- | ------- |
| Search by IP or domain only | Complex queries with various search arguments |
| Returns specific data for an IP or domain | Returns multiple results based on the query |
| Aggregated data from all data collections | Specific data from chosen data collections or indices |
| Corresponds to IP/Domain Info tool | Corresponds to Responses, DNS, WHOIS, SSL certs search |

__Use Cases for Host API__

- **SIEM System Plugin**: Automatically enrich security logs in any SIEM system with detailed IP/domain data from Netlas, providing contextual insights for security events.
- **Firewall Management Plugin**: Develop a plugin for firewall solutions that utilizes Netlas data to automate security rule creation based on IP reputation, known threats, and geolocation.
- **CMS Plugin**: Identify a site visitor’s company by IP address for lead generation, analytics, or content adaptation.
- **Remote Employee Compliance Checks**: Verify the IP address information of remote employees to ensure they connect from secure locations.
- **Browser Plugin**: Access security-related information about web resources and their hosting servers while browsing.

__Use Cases for Search Query API__

- **Reconnaissance Script**: Automate the collection and analysis of external attack surface data to understand the target environment.
- **Non-intrusive Assessment**: Gather information about a system's security posture without direct interaction, avoiding detection.
- **OSINT Profiling**: Collect publicly available information to create detailed profiles of organizations or systems.
- **Malware Resources Search**: Identify and track potential threats by searching for online resources that host malware.
- **Phishing Resources Search**: Identify and analyze websites used in phishing campaigns to help protect against fraudulent activities.

## Host API

| HTTP Method   | Endpoint              | Description                                                              |
|---------------|-----------------------|--------------------------------------------------------------------------|
| `GET`         | /host/`{host}`/       | Summary of data for any host, including DNS records, WHOIS info & scans. |

You can fetch data for any valid IP address or domain name. This API endpoint aggregates data from all available data collections related to the queried IP or domain.

=== "Bash"
    ``` sh title="Query using CLI tool"
    # IP query example
    netlas host "135.125.237.168" --apikey "YOUR_API_KEY" --format json

    # Domain query example
    netlas host "app.netlas.io" --apikey "YOUR_API_KEY" --format json
    ```

=== "Curl"
    ``` sh title="Query using curl utility"
    # IP query example
    curl -X 'GET' "https://app.netlas.io/api/host/135.125.237.168/" \
      -H "X-API-Key: YOUR_API_KEY"

    # Domain query example
    curl -X 'GET' "https://app.netlas.io/api/host/app.netlas.io/" \
      -H "X-API-Key: YOUR_API_KEY"
    ```

=== "Python"
    ``` py title="Query using Python SDK"
    import netlas
    import json

    # Define the API key and initialize the Netlas client
    netlas_connection = netlas.Netlas(api_key="YOUR_API_KEY")

    # IP query example
    ip_info = netlas_connection.host(host="135.125.237.168")
    print(json.dumps(ip_info))

    # Domain query example
    domain_info = netlas_connection.host(host="app.netlas.io")
    print(json.dumps(domain_info))
    ```

A JSON document with the following data will be returned:

| IP query | Domain query |
| -------- | ------- |
| PTR, Subnet and Autonomous system data  | DNS records |
| Geolocation | Related domains |
| Privacy flags (VPN/Tor/Proxy) |  Domain WHOIS data   |
| Organization    | Threat intelligence data |
| IP WHOIS data | Exposed WEB ports & software, including detected CVE |
| Related domains |  |
| Threat intelligence data | |
| Exposed ports & software, including detected CVE |  |

??? abstract "JSON documents with IP/domain information"

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

## Filtering Output

You can limit the amount of output data by using the `include` option to specify a list of fields to be included in the response, or `exclude` to filter out certain fields. However, both options cannot be used simultaneously in the same request.

=== "Bash"

    ``` sh title="Filtering the output with query options"
    netlas host "google.com" \
      --format json \
      --include "whois.registrant.organization" \
      --apikey "YOUR_API_KEY"
    ```

    <div class="result" markdown>

    ``` { .json .no-copy }
    {"whois": {"registrant": {"organization": "Google LLC"}}}
    ```

    </div>

=== "Curl"

    ``` sh title="Filtering the output with query options"
    curl -X 'GET' -s \
    "https://app.netlas.io/api/host/google.com/?fields=whois.registrant.organization&source_type=include" \
      -H "X-API-Key: YOUR_API_KEY"
    ```

    <div class="result" markdown>

    ``` { .json .no-copy }
    {"whois": {"registrant": {"organization": "Google LLC"}}}
    ```

    </div>

=== "Python"

    ``` py title="Filtering the output with query options"
    import netlas
    import json
    
    # Define the API key and initialize the Netlas client
    netlas_connection = netlas.Netlas(api_key="YOUR_API_KEY")

    domain_info = netlas_connection.host(
      host="google.com", 
      fields="whois.registrant.organization"
    )

    print(json.dumps(domain_info))
    ```

    <div class="result" markdown>

    ``` { .json .no-copy }
    {"whois": {"registrant": {"organization": "Google LLC"}}}
    ```

    </div>

Use `jq` utility to filter CLI tool and `curl` output.

=== "Bash"

    ``` sh title="Filtering the output with jq utility"
    netlas host "8.8.8.8" -f json -a "YOUR_API_KEY" | jq -r ".ptr[0]"
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    dns.google
    ```

    </div>

=== "Curl"

    ``` sh title="Filtering the output with jq utility"
    curl -X 'GET' -s \
      "https://app.netlas.io/api/host/8.8.8.8/" \
      -H "X-API-Key: YOUR_API_KEY" \
    | jq -r ".ptr[0]"
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    dns.google
    ```

    </div>

=== "Python"

    ``` py title="Filtering the output"
    import netlas

    # Define the API key and initialize the Netlas client
    netlas_connection = netlas.Netlas(api_key="YOUR_API_KEY")

    ip_info = netlas_connection.host(host="8.8.8.8")
    print(ip_info["ptr"][0])
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    dns.google
    ```

    </div>

Both filtering methods can also be used for the Search Query API.

### YAML Output

By default, the CLI tool outputs data in YAML format, which is more readable for humans. Additionally, Netlas employs the [Pygments library](https://pygments.org) for YAML syntax highlighting. However, using YAML in automation scripts is not recommended for two main reasons:

1. **Complex Data Parsing:** Extracting specific data from YAML is more challenging compared to JSON due to its structured complexity.
2. **Additional Formatting Handling:** ANSI escape codes used for formatting and color in the CLI output must be filtered out, adding an extra step in data processing.

This is why we use the `--format json` option with the CLI tool in the examples provided. If you still need to utilize YAML output, you can strip ANSI escape characters using the `sed` utility:

``` bash title="Filtering CLI tool formated YAML output"
netlas count "host:135.125.237.168" | sed "s/\x1b\[[0-9;]*m//g"
```

Explanation:

- `\x1b`: This is the escape character, indicating the start of an ANSI sequence.
- `\[[0-9;]*m`:  This pattern matches the sequence starting with `[`, followed by any combination of digits and semicolons, and ending with `m`, which is typical for color and style escape codes.

## Search Query API

Each Netlas Search tool is associated with a set of API endpoints that facilitate different search operations:

| HTTP Method | Endpoint                        | Description                                                                                                           |
|-------------|---------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| `GET`         | /api/[search_tool]/             | Perform a search query, returning up to 20 results per request.                                                      |
| `POST`        | /api/[search_tool]/download/    | Fetch search results in a stream format. Specify the stream size as a parameter in the request.                      |
| `GET`         | /api/[search_tool]_count/       | Calculate the total number of results for a specific query. Returns exact counts for <1000 results; otherwise, an estimate with ≤3% margin of error. |
| `GET`         | /api/[search_tool]_facet/       | Organize search results into groups based on specified criteria (`missing in Certificates Search`).                                    |
| `GET`         | /api/[search_tool]_summary/     | Provide a summary of search results for the given query (`Responses & DNS search only`).                                                             |

__Available Search Tools__
Replace `[search_tool]` in the endpoints above with one of the following tools:

- `responses` – Search for responses data.
- `domains` – Search for DNS-related data.
- `whois_ip` – Search for IP WHOIS information.
- `whois_domains` – Search for domain WHOIS information.
- `certs` – Search for certificates data.

A complete list of available endpoints is detailed in the [API specification](https://app.netlas.io/schema/).

Regardless of the specific search tool being used, endpoints of the same category exhibit consistent behavior. Both the Netlas SDK and the CLI Tool provide corresponding methods and commands to interact with these endpoints effectively.

=== "Bash"

    ``` sh title="Responses available for the query, Search command"
    #!/bin/bash

    # Define the API key
    API_KEY="YOUR_API_KEY"

    # Define the query
    QUERY="host:135.125.237.168"

    # Get the count of responses for the given query
    COUNT=$(netlas count "$QUERY" --apikey "$API_KEY" --format json \
        | jq ".count")

    # Check if there are any responses and search for them if there are
    if [ "$COUNT" -gt 0 ]; then
        echo "Responses for $QUERY"

        # Calculate the number of pages needed based on 20 results per page
        NUM_PAGES=$((($COUNT + 19) / 20))

        # Loop through each page and fetch the results
        for ((PAGE=0; PAGE<NUM_PAGES; PAGE++)); do
            RESULTS=$(netlas search "$QUERY" --datatype "response" \
                --page "$PAGE" --include "uri" \
                --apikey "$API_KEY" --format json)
            
            # Check if items exist in the results and print URIs
            if [[ $RESULTS == *"items"* ]]; then
                echo "$RESULTS" | jq -r ".items[].data.uri"
            fi

            # Wait for a second to avoid throttling
            sleep 1
        done
    else
        echo "No responses found."
    fi
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    Responses for host:135.125.237.168
    smtpstarttls://135.125.237.168:25
    smtps://135.125.237.168:465
    https://135.125.237.168:443/
    imap://135.125.237.168:143
    imaps://135.125.237.168:993
    smtp://135.125.237.168:25
    http://135.125.237.168:80/
    imapstarttls://135.125.237.168:143
    ```

    </div>

=== "Curl"

    ``` sh title="Responses available for the query, Search endpoint"
    #!/bin/bash

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"

    # Define the query
    QUERY="host:135.125.237.168"

    # Get the count of responses for the given query
    COUNT=$(curl -s -H "X-API-Key: $API_KEY" \
        "$BASE_URL/responses_count/?q=$QUERY" \
        | jq ".count")

    # Check if there are any responses and search for them if there are
    if [ "$COUNT" -gt 0 ]; then
        echo "Responses for $QUERY"

        # Calculate the number of pages needed based on 20 results per page
        NUM_PAGES=$((($COUNT + 19) / 20))

        # Loop through each page and fetch the results
        for ((PAGE=0; PAGE<NUM_PAGES; PAGE++)); do
            OFFSET=$(($PAGE*20))
            RESULTS=$(curl -s -H "X-API-Key: $API_KEY" \
                "$BASE_URL/responses/?q=$QUERY&start=$OFFSET&fields=uri" \
                | jq -r ".items[].data.uri")

            # Check if items exist in the results and print URIs
            if [ -n "$RESULTS" ]; then
                echo "$RESULTS"
            fi

            # Wait for a second to avoid throttling
            sleep 1
        done
    else
        echo "No responses found."
    fi
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    Responses for host:135.125.237.168
    smtpstarttls://135.125.237.168:25
    smtps://135.125.237.168:465
    https://135.125.237.168:443/
    imap://135.125.237.168:143
    imaps://135.125.237.168:993
    smtp://135.125.237.168:25
    http://135.125.237.168:80/
    imapstarttls://135.125.237.168:143
    ```

    </div>

=== "Python"

    ``` py title="Responses available for the query, Search method"

    import netlas
    import time

    # Define the API key and initialize the Netlas client
    netlas_connection = netlas.Netlas(api_key="YOUR_API_KEY")

    # Define the query
    query = "host:135.125.237.168"

    # Get the count of responses for the given query
    cnt_of_res = netlas_connection.count(query=query, datatype="response")

    # Check if there are any responses and search for them if there are
    if cnt_of_res["count"] > 0:
        print("Responses for " + query)

        # Calculate the number of pages needed based on 20 results per page
        num_pages = (cnt_of_res["count"] + 19) // 20  # rounding up

        # Loop through each page and fetch the results
        for page in range(num_pages):
            search_results = netlas_connection.search(
              query=query, datatype="response", page=page, fields="uri"
              )

            if "items" in search_results:
                # iterate over data and print: IP address, port, path and protocol
                for response in search_results["items"]:
                    print(response["data"]["uri"])
            
            # wait for a second to avoid throtling
            time.sleep(1)
    else:
        print("No responses found.")

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    Responses for host:135.125.237.168
    smtpstarttls://135.125.237.168:25
    smtps://135.125.237.168:465
    https://135.125.237.168:443/
    imap://135.125.237.168:143
    imaps://135.125.237.168:993
    smtp://135.125.237.168:25
    http://135.125.237.168:80/
    imapstarttls://135.125.237.168:143
    ```

    </div>

### Search vs. Download

The **Search** and **Download** methods in the Netlas API cater to different needs, primarily based on the volume of data required:

- **Search Method**: Depending on your pricing plan, you can retrieve between 200 and 4000 search results. This method requires making a separate request for every 20 results, which is ideal for situations where pagination is needed or when the expected number of results is relatively low. This approach is used in our web application to support user interfaces that include pagination.

- **Download Method**: Unlike the Search method, the Download method is not technically limited to a specific number of results per request. Instead, it is governed only by your pricing plan, allowing for the retrieval of large data sets in a single request. This method is particularly useful for handling extensive search queries that yield a large number of results, eliminating the need for repeated requests with incremental offsets. The Netlas SDK and CLI tool additionally include a `download_all()` method and an `--all` key that allow you to query all available results.

Here's an example illustrating the simplicity and conciseness of using the Download method:

=== "Bash"

    ``` sh title="Responses available for the query, download command"
    #!/bin/bash

    # Define the API key
    API_KEY="YOUR_API_KEY"

    # Define the query
    QUERY="host:*.netlas.io"

    # Download all available responses
    netlas download "$QUERY" --all --include "uri" --apikey "$API_KEY"\
      | jq -r ".data.uri"
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    Responses for host:*.netlas.io
    http://pay.netlas.io:80/
    https://pay.netlas.io:443/login
    https://pay.netlas.io:443/
    http://mail.netlas.io:80/
    https://mail.netlas.io:443/
    http://app.netlas.io:80/
    https://app.netlas.io:443/
    ```

    </div>

=== "Curl"

    ``` sh title="Responses available for the query, download endpoint"
    #!/bin/bash

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"

    # Define the query
    QUERY="host:*.netlas.io"

    # Get the count of responses for the given query
    COUNT=$(curl -s -H "X-API-Key: $API_KEY" \
        "$BASE_URL/responses_count/?q=$QUERY" \
        | jq ".count")

    # Check if there are any responses and download them if there are
    if [ "$COUNT" -gt 0 ]; then
        curl -s -X POST "$BASE_URL/responses/download/" \
        -H "Content-Type: application/json" \
        -H "X-API-Key: $API_KEY" \
        -d '{"q": "'"${QUERY}"'",
              "fields": ["uri"],
              "source_type": "include",
              "size": '"${COUNT}"'}' \
        | jq -r ".[].data.uri"
    fi
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    Responses for host:*.netlas.io
    http://pay.netlas.io:80/
    https://pay.netlas.io:443/login
    https://pay.netlas.io:443/
    http://mail.netlas.io:80/
    https://mail.netlas.io:443/
    http://app.netlas.io:80/
    https://app.netlas.io:443/
    ```

    </div>

=== "Python"

    ``` py title="Responses available for the query, download_all() method"

    import netlas
    import json

    # Define the API key and initialize the Netlas client
    netlas_connection = netlas.Netlas(api_key="YOUR_API_KEY")

    # Define the query
    query = "host:*.netlas.io"

    # Download all available responses
    for resp in netlas_connection.download_all(query):
        # decode from binary stream
        response = json.loads(resp.decode("utf-8"))
        print(response["data"]["uri"])
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    Responses for host:*.netlas.io
    http://pay.netlas.io:80/
    https://pay.netlas.io:443/login
    https://pay.netlas.io:443/
    http://mail.netlas.io:80/
    https://mail.netlas.io:443/
    http://app.netlas.io:80/
    https://app.netlas.io:443/
    ```

    </div>

The code utilizing the **Download** method is simpler and more concise because it eliminates the need for looped requests with offsets; all results are obtained in a single request.

!!! question "How to download large sets of search results that exceed download limits? [Read the FAQ &rarr;](../support.md#download-limits)"

### Indices

By default, queries target the most up-to-date index. However, other indices may also be available to you. Utilizing indices through the API is analogous to [using indices in the web application](../search/search_basics.md#historical-search).

=== "Bash"

    ``` sh title="Available responses indices"
    netlas indices --format json --apikey "YOUR_API_KEY" \
        | jq -r '.[] | select(.type | contains("responses")) | "\(.id): \(.name)"' \
        | sort -g
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    83: responses-2022-09-21
    84: responses-2023-01-03
    86: responses_*_2023-05-11
    89: responses_*_2023-09-22
    103: responses_*_2024-01-22
    110: responses_*_2024-04-25
    ```

    </div>

=== "Curl"

    ``` sh title="Available responses indices"
    #!/bin/bash

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"

    # Fetch the indices and filter the results
    curl -s -H "X-API-Key: $API_KEY" "$BASE_URL/indices/" \
        | jq -r '.[] | select(.type | contains("responses")) | "\(.id): \(.name)"' \
        | sort -g
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    83: responses-2022-09-21
    84: responses-2023-01-03
    86: responses_*_2023-05-11
    89: responses_*_2023-09-22
    103: responses_*_2024-01-22
    110: responses_*_2024-04-25
    ```

    </div>

=== "Python"

    ``` py title="Available responses indices"

    import netlas

    # Define the API key and initialize the Netlas client
    api_key = "YOUR_API_KEY"
    netlas_connection = netlas.Netlas(api_key=api_key)

    # Fetch the indices
    indices = netlas_connection.indices()

    # Filter indices where the type contains "responses" and sort by id
    filtered_indices = [index for index in indices if "responses" in index["type"]]
    sorted_indices = sorted(filtered_indices, key=lambda x: x["id"])

    # Print the results
    for index in sorted_indices:
        print(f"{index["id"]}: {index["name"]}")

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    83: responses-2022-09-21
    84: responses-2023-01-03
    86: responses_*_2023-05-11
    89: responses_*_2023-09-22
    103: responses_*_2024-01-22
    110: responses_*_2024-04-25
    ```

    </div>

One of the examples provided below demonstrates how to perform a historical search and compare results from different indices.

## Rate Limits

__Error 429: Too Many Requests__

The `HTTP 429 (Too Many Requests)` error indicates that the user has exceeded the allowed request rate within a specified time frame. Netlas enforces rate limits to ensure fair resource distribution and maintain system stability.

**Possible Causes**

- Sending requests at a higher frequency than permitted by the API rate limits.
- Running multiple concurrent API calls that collectively exceed the allowed threshold.
- Using automated scripts or bots without implementing rate-limiting mechanisms.

!!! info "Rate Limits"

    The Search Tools API enforces two distinct rate limits to ensure fair usage and optimal performance:
    
    - For general API operations (excluding certificate searches): **60 requests per minute**.
    - For search and count operations involving certificates: **3 requests per minute**.
    
    If these limits are exceeded, the system may temporarily restrict access to the API to maintain service stability. Users experiencing such restrictions should adjust their request frequency accordingly.

**How to Resolve**

- Reduce Request Frequency: Ensure that your API calls adhere to the specified rate limits.
- Implement Backoff Strategies: If a **429 error** occurs, wait before retrying. A common approach is exponential backoff, where each retry waits progressively longer.
- Check Headers for Retry Information: The API may include a `Retry-After` header, specifying how long to wait before sending new requests.
- Optimize Queries: Minimize redundant requests by refining search parameters or using batch queries where applicable.
- Upgrade Subscription Plan: If your use case requires a higher request limit, consider switching to a plan with increased API quotas.

!!! tip "Rate Limit Handling in SDK and CLI:"
    - When working with Netlas via SDK or CLI, a simple way to prevent hitting rate limits is to introduce a small delay between requests.
    - **For CLI (single-threaded mode)**: Insert a `time.sleep(1)` before each API request to ensure that no more than one request per second is sent.
    - **For SDK-based implementations**: Implement a similar delay in your request loop to avoid exceeding rate limits.

__Practices for Addressing Rate Limiting Issues__

Here is an improved example of handling `HTTP 429 errors` in Python, with separate implementations using the **Requests** library and the **Netlas SDK**. These scripts include enhanced error handling, logging, and a limit on the number of retry attempts.

=== "Requests"
    ```python title="Python Requests library script handling Rate Limit Error"
    import time
    import requests

    API_URL = "https://app.netlas.io/api/certs_count/?q=certificate.validity.start%3A%222025-02-10%22"
    HEADERS = {
        "accept": "application/json",
        "X-API-Key": "your_api_key",
        "X-CSRFTOKEN": "your_csrf_token"
    }
    MAX_RETRIES = 5  # Maximum number of attempts
    DEFAULT_WAIT = 10  # Default waiting time (seconds)

    def fetch_data_requests():
        retries = 0
        while retries < MAX_RETRIES:
            try:
                response = requests.get(API_URL, headers=HEADERS)
                if response.status_code == 200:
                    return response.json()

                elif response.status_code == 429:
                    retry_after = int(response.headers.get("Retry-After", DEFAULT_WAIT))
                    print(f"[Requests] Rate limit exceeded. Retrying in {retry_after} seconds...")
                    time.sleep(retry_after)
                    retries += 1
                else:
                    print(f"[Requests] Unexpected status code: {response.status_code}")
                    return None
            except requests.RequestException as e:
                print(f"[Requests] Request failed: {e}")
                time.sleep(DEFAULT_WAIT)
                retries += 1

        print("[Requests] Max retries reached. Exiting.")
        return None

    # Executing the request via Requests lib
    data_requests = fetch_data_requests()
    print("Requests Data:", data_requests)
    ```

=== "Netlas SDK"
    ```python title="Netlas Python SDK script handling Rate Limit Error"
    import time
    from netlas import Netlas

    API_KEY = "your_api_key"
    MAX_RETRIES = 5  # Maximum number of attempts
    DEFAULT_WAIT = 10  # Default waiting time (seconds)

    def fetch_data_netlas():
        netlas = Netlas(API_KEY)
        retries = 0
        while retries < MAX_RETRIES:
            try:
                response = netlas.query('certificate.validity.start:"2025-02-10"', "certs_count")
                if response.status_code == 200:
                    return response.json()

                elif response.status_code == 429:
                    retry_after = int(response.headers.get("Retry-After", DEFAULT_WAIT))
                    print(f"[Netlas SDK] Rate limit exceeded. Retrying in {retry_after} seconds...")
                    time.sleep(retry_after)
                    retries += 1
                else:
                    print(f"[Netlas SDK] Unexpected status code: {response.status_code}")
                    return None
            except Exception as e:
                print(f"[Netlas SDK] Request failed: {e}")
                time.sleep(DEFAULT_WAIT)
                retries += 1

        print("[Netlas SDK] Max retries reached. Exiting.")
        return None

    # Executing the request via Netlas SDK
    data_netlas = fetch_data_netlas()
    print("Netlas SDK Data:", data_netlas)
    ```

!!! note ""
    When a **429 Too Many Requests** error occurs, the API response includes a `Retry-After` header that specifies the number of seconds to wait before making another request.

__Error 1006: Temporary Block__

If an account is temporarily blocked due to excessive request frequency or repeated request errors, the API will respond with **Error 1006** for all subsequent calls.

**Possible Causes**

- Continuously exceeding the allowed number of requests per minute.
- Sending numerous improperly formatted or invalid API requests.
- Making unauthorized attempts to access restricted features.

**How to Resolve**  

- The block is temporary and will be lifted automatically after a predefined cooldown period. The duration of the block depends on the severity of the violation.
- Ensure that future API requests comply with the defined rate limits and formatting requirements.
- Regularly monitor your API usage to avoid unintentionally breaching the restrictions.
- If you believe your account was mistakenly blocked or need further clarification, contact [support](mailto:support@netlas.io).

!!! warning "Blocking Policy"

    To prevent abuse and ensure fair API usage across all users, the system may temporarily block access under certain conditions:
    
    - Repeatedly exceeding the defined rate limits.
    - Sending a high volume of malformed or invalid requests.
    - Attempting to bypass subscription-based restrictions or engaging in suspicious activity.
    
    If a block is triggered, all subsequent API requests will be denied until the restriction is lifted.

## Error Handling

This section provides an overview of common error responses, their meanings, and best practices for resolving them. Understanding these error messages will help you troubleshoot issues effectively and optimize your API usage.

__Error 400: Request Parsing Error__

This error occurs when the API server is unable to process the request due to improperly structured or invalid input data. It indicates that the request does not meet the expected format or is missing essential components.

**Possible Causes**  

- Incorrectly formatted JSON.
- The use of invalid or unrecognized parameter names.
- Missing essential fields required for the request to be processed successfully.
- Providing values of incorrect data types.

**How to Resolve**

- Carefully review the API documentation to ensure that all request parameters are correctly structured and formatted.
- Double-check the JSON syntax to ensure there are no missing brackets, commas, or quotation marks.
- Use the [API Schema](https://app.netlas.io/schema/) as a reference to validate the structure of your request before sending.
- If you continue to experience issues, consider testing your request with a minimal valid payload and incrementally adding parameters to identify the root cause.

__Error 400: Feature Not Available in Current Subscription Plan__

This error indicates that the requested feature, search capability, or data access level is not included in your current subscription tier. Some functionalities are available only to users with upgraded plans that offer extended API access.

**Possible Causes**  

- You are attempting to use a search filter, aggregation, or data set restricted to higher-tier plans.
- The number of API queries allowed per day under your current plan has been reached.
- The feature you are trying to access is considered premium and requires an advanced subscription.

**How to Resolve**  

- Check your current subscription plan and review the list of available features.
- If the required feature is included in a higher-tier plan, consider upgrading to gain access.
- Contact [support](mailto:support@netlas.io) if you believe this restriction has been applied incorrectly or need assistance selecting the appropriate plan.

__Error 400: Exceeded Maximum Allowed Queries__

Some subscription plans impose a cap on the number of queries a user can execute within a defined period. When this limit is exceeded, additional requests will be rejected, and this error will be returned.

**Possible Causes**

- The number of queries sent has exceeded the limit set by your subscription plan.
- Multiple applications or users are making API requests simultaneously, increasing the total query count.
- A script or automated process is running excessive requests in a short period.
- Your API key may be used in unauthorized or unintended requests, leading to a higher query count.

**How to Resolve**  

- Wait until your query limit resets according to your plan’s specifications.
- If your workload requires more frequent API calls, explore higher-tier subscription options with increased query allowances.
- Optimize your search queries to retrieve the necessary data more efficiently, reducing redundant or unnecessary requests.

__Error 403: Subscription Plan Restriction__

This error occurs when a user attempts to perform an operation that is not permitted under their current subscription plan. Certain features and query limits vary depending on the chosen plan, and exceeding these restrictions will result in this error.

**Possible Causes**

- Conducting certificate searches when this feature is not included in the plan.
- Exceeding the number of stored searches or queries allowed per day.
- Using advanced query parameters that are restricted to premium accounts.

**How to Resolve**  

- Review your subscription plan details to understand its limitations.
- If necessary, upgrade to a plan that provides the required level of access.
- Visit the [pricing and feature comparison page](https://app.netlas.io/plans/) for a detailed breakdown of available plans and their features.

## Examples

The examples below assume that the API key has been saved by the user, as [described](setup.md#setting-up-api-key) in the SDK installation instructions.

### IP to Company

In the example below, the Host API is used to query for the organization name and country associated with IP addresses.

=== "Bash"

    ``` sh title="ip2company-cli.sh"
    #!/bin/bash

    # Check if an IP address is provided
    if [ "$#" -ne 1 ]; then
        echo "Usage: $0 <IP_ADDRESS>"
        exit 1
    fi

    # Make the API call using CLI tool, assuming that API key is saved
    response=$(netlas host "$1" --format json)

    # Extract organization and country using jq
    organization=$(echo $response | jq -r '.organization // "n/a"')
    country=$(echo $response | jq -r '.geo.country // "n/a"')

    # Conditional output based on data availability
    if [ "$organization" = "n/a" ] && [ "$country" = "n/a" ]; then
        echo "No organization information found for the IP: $1"
    else
        echo "$organization, $country"
    fi
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash ip2company-cli.sh 8.8.8.8        
    Google LLC (GOGL), United States
    ```

    </div>

=== "Curl"

    ``` sh title="ip2company-curl.sh"
    #!/bin/bash

    # Check if an IP address is provided
    if [ "$#" -ne 1 ]; then
        echo "Usage: $0 <IP_ADDRESS>"
        exit 1
    fi

    # Define the API key and the URL for query
    API_KEY="YOUR_API_KEY"
    API_URL="https://app.netlas.io/api/host/$1/"

    # Make the API call using curl with the API key in the header
    response=$(curl -s -H "X-API-Key: $API_KEY" "$API_URL")

    # Extract organization and country using jq
    organization=$(echo "$response" | jq -r '.organization // "n/a"')
    country=$(echo "$response" | jq -r '.geo.country // "n/a"')

    # Conditional output based on data availability
    if [ "$organization" = "n/a" ] && [ "$country" = "n/a" ]; then
        echo "No organization information found for the IP: $1"
    else
        echo "$organization, $country"
    fi
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash ip2company-curl.sh 8.8.8.8        
    Google LLC (GOGL), United States
    ```

    </div>

=== "Python"

    ``` py title="ip2company.py"
    import sys
    import netlas

    # Check if an IP address is provided
    if len(sys.argv) < 2:
        print("Usage: python ip2company.py <IP_ADDRESS>")
        sys.exit(1)

    # Initialize the Netlas API assuming that API key is saved
    api_key = netlas.helpers.get_api_key()
    netlas_api = netlas.Netlas(api_key=api_key)

    # Make the API call to get host information
    try:
        response = netlas_api.host(host=sys.argv[1])

        # Check if the response contains the necessary data
        if response and "organization" in response:
            organization_name = response.get("organization", "n/a")
            country = response.get("geo", {}).get("country", "n/a")
            # Default to "n/a" if not available
            
            print(f"{organization_name}, {country}")
        else:
            print("No organization information found for the IP:", sys.argv[1])
    except Exception as e:
        print("An error occurred:", str(e))

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # python ip2company.py 8.8.8.8        
    Google LLC (GOGL), United States

    ```

    </div>

### Abuse Contact

This script retrieves the abuse contact for a domain or IP address using the Host API. For domains, it uses the registrar's email, and for IP addresses, it retrieves the abuse email from the IP WHOIS data.

??? note "Please note, data availability depends on your pricing plan"

    If your pricing plan does not include access to contact information such as phone numbers and email addresses, this example will not be applicable.

=== "Bash"

    ``` sh title="abuse_contact-cli.sh"
    #!/bin/bash

    # Check if a domain or IP address is provided
    if [ "$#" -ne 1 ]; then
        echo "Usage: $0 <DOMAIN_OR_IP>"
        exit 1
    fi

    # Make the API call using CLI tool, assuming that API key is saved
    response=$(netlas host $1 --format json)

    # Determine the type and extract the relevant abuse contact
    if echo "$response" | jq -e ".ip" > /dev/null; then
        abuse_contact=$(echo "$response" | jq -r '.whois.abuse // "n/a"')
    elif echo "$response" | jq -e ".domain" > /dev/null; then
        abuse_contact=$(echo "$response" | jq -r '.whois.registrar.email // "n/a"')
    else
        abuse_contact="n/a"
    fi

    echo "$abuse_contact"

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash abuse_contact-cli.sh dns.google
    abusecomplaints@markmonitor.com
    ~ # bash abuse_contact-cli.sh 8.8.8.8   
    network-abuse@google.com
    ```

    </div>

=== "Curl"

    ``` sh title="abuse_contact-curl.sh"
    #!/bin/bash

    # Check if a domain or IP address is provided
    if [ "$#" -ne 1 ]; then
        echo "Usage: $0 <DOMAIN_OR_IP>"
        exit 1
    fi

    # Define the API key and the URL for query
    API_KEY="YOUR_API_KEY"
    API_URL="https://app.netlas.io/api/host/$1/"

    # Make the API call using curl with the API key in the header
    response=$(curl -s -H "X-API-Key: $API_KEY" "$API_URL")

    # Determine the type and extract the relevant abuse contact
    if echo "$response" | jq -e '.ip' > /dev/null; then
        abuse_contact=$(echo "$response" | jq -r '.whois.abuse // "n/a"')
    elif echo "$response" | jq -e '.domain' > /dev/null; then
        abuse_contact=$(echo "$response" | jq -r '.whois.registrar.email // "n/a"')
    else
        abuse_contact="n/a"
    fi

    echo "$abuse_contact"
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash abuse_contact-curl.sh dns.google
    abusecomplaints@markmonitor.com
    ~ # bash abuse_contact-curl.sh 8.8.8.8   
    network-abuse@google.com
    ```

    </div>

=== "Python"

    ``` py title="abuse_contact.py"
    import sys
    import netlas

    # Check if a domain or IP address is provided
    if len(sys.argv) < 2:
        print("Usage: python abuse_contact.py <DOMAIN_OR_IP>")
        sys.exit(1)

    # Initialize the Netlas API, assuming that API key is saved
    api_key = netlas.helpers.get_api_key()
    netlas_api = netlas.Netlas(api_key=api_key)

    # Make the API call to get host information
    try:
        response = netlas_api.host(host=sys.argv[1])

        # Determine if the identifier is a domain or IP and extract the relevant abuse contact
        if "domain" in response and "whois" in response:
            if "registrar" in response["whois"] and "email" in response["whois"]["registrar"]:
                abuse_contact = response["whois"]["registrar"]["email"]
            else:
                 abuse_contact = "n/a"
        elif "ip" in response and "whois" in response:
            abuse_contact = response["whois"]["abuse"] if "abuse" in response["whois"] else "n/a"
        else:
            abuse_contact = "n/a"

        print(abuse_contact)
    except Exception as e:
        print("An error occurred: ", str(e))
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # python abuse_contact.py dns.google
    abusecomplaints@markmonitor.com
    ~ # python abuse_contact.py 8.8.8.8   
    network-abuse@google.com
    ```

    </div>

### Subdomain Enumeration

This example leverages the Search Query API to access the DNS Registry data collection. It performs a search based on user input and downloads all available results.

=== "Bash"

    ``` sh title="subdomains-cli.sh"
    #!/bin/bash

    # Check if a domain search query is provided
    if [ "$#" -ne 1 ]; then
        printf "Usage:\n\t$0 \"<*.domain.com>\"\n\t$0 \"<domain.*>\"\n"
        exit 1
    fi

    # Define the API key
    API_KEY="YOUR_API_KEY"

    # Download all available results
    netlas download "domain:$1" --all --datatype domain --include "domain" --apikey "$API_KEY" \
        | jq -r ".data.domain"
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash subdomains-cli.sh "*.netlas.io"
    app.netlas.io
    pay.netlas.io
    www.netlas.io
    mail.netlas.io
    ```

    </div>

=== "Curl"

    ``` sh title="subdomains-curl.sh"
    #!/bin/bash

    # Check if a domain search query is provided
    if [ "$#" -ne 1 ]; then
        printf "Usage:\n\t$0 \"<*.domain.com>\"\n\t$0 \"<domain.*>\"\n"
        exit 1
    fi

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"

    # Get the count of domains for the given query
    COUNT=$(curl -s -H "X-API-Key: $API_KEY" \
        "${BASE_URL}/domains_count/?q=domain:$1" | jq ".count")

    # Check if there are any results and download them if there are
    if [ "$COUNT" -gt 0 ]; then
        curl -s -X POST "$BASE_URL/domains/download/" \
            -H "Content-Type: application/json" \
            -H "X-API-Key: $API_KEY" \
            -d "{
                \"q\": \"domain:$1\",
                \"fields\": [\"domain\"],
                \"source_type\": \"include\",
                \"size\": $COUNT
            }" | jq -r ".[].data.domain"
    else
        echo "No results found for the query."
    fi
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash subdomains-curl.sh "*.netlas.io"
    app.netlas.io
    pay.netlas.io
    www.netlas.io
    mail.netlas.io
    ```

    </div>

=== "Python"

    ``` py title="subdomains.py"
    import sys
    import netlas
    import json

    # Check if a domain search query is provided
    if len(sys.argv) != 2:
        print(
            "Usage:\n"
            "\tpython subdomains.py \"<*.domain.com>\"\n"
            "\tpython subdomains.py \"<domain.*>\""
        )
        sys.exit(1)

    # Initialize the Netlas API assuming that API key is saved
    api_key = netlas.helpers.get_api_key()
    netlas_api = netlas.Netlas(api_key=api_key)

    # Define the query
    query = "domain:" + sys.argv[1]

    try:
        # Download all available results
        for resp in netlas_api.download_all(
            query=query,
            datatype="domain",
            fields="domain"
        ):
            # decode from binary stream
            response = json.loads(resp.decode("utf-8"))
            print(response["data"]["domain"])
    except Exception as e:
        print("An error occurred:", str(e))
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # python subdomains.py "*.netlas.io"
    app.netlas.io
    pay.netlas.io
    www.netlas.io
    mail.netlas.io
    ```

    </div>

!!! tip "Always choose the download method to fetch a large amount of results."

    This example uses the Download method to accommodate potentially large datasets that could exceed the 4000 result limit imposed by the Search method.

### Related Domains

This script takes a domain as input, queries Domain WHOIS data for the organization name of a registrant, and searches for domains with the same organization name. If the number of search results exceeds 1,000, user confirmation is required to proceed with the download. Use `-f` or `--force` to bypass the confirmation dialogue.

=== "Bash"

    ``` sh title="related_domains-cli.sh"
    #!/bin/bash

    # Check if a domain is provided
    if [ "$#" -lt 1 ]; then
        echo "Usage: $0 <DOMAIN> [-f|--force]"
        echo "    <DOMAIN> - the domain to query"
        echo "    -f, --force - force downloading all results without prompt, even if results are more than 1000"
        exit 1
    fi

    force_download=false
    if [ "$#" -eq 2 ] && ([[ "$2" == "-f" ]] || [[ "$2" == "--force" ]]); then
        force_download=true
    fi

    # Fetch organization name from WHOIS data
    organization=$(netlas search "domain:$1" \
            --format json --datatype whois-domain \
            | jq -r ".items[0].data.registrant.organization")

    if [ "$organization" == "null" ] || [ -z "$organization" ]; then
        echo "No organization found for the domain."
        exit 1
    fi

    # Create query
    domains_query='registrant.organization:\"$organization\"'

    #Count domains with the same organization name
    count=$(netlas count "$domains_query" \
            --format json --datatype whois-domain | jq ".count")

    # Check if there are any results and optionally ask for confirmation
    if [ "$count" -eq 0 ]; then
        echo "No related domains found."
        exit 1
    elif [ "$count" -gt 1000 ] && ! $force_download; then
        echo "Found $count results for organization: $organization."
        read -p "Do you really want to download all results? (Y/n): " user_input
        if [[ "$user_input" != "Y" && "$user_input" != "y" ]]; then
            echo "Download canceled."
            exit 0
        fi
    fi

    # Fetching domains registered to the same organization
    netlas download "$domains_query"  \
            --datatype whois-domain --include "domain" \
            --count "$count" \
            | jq -r ".data.domain"
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash related_domains-cli.sh microsoft.com 
    Found 33304 results for organization: Microsoft Corporation.
    Do you really want to download all results? (Y/n): Y
    halo5.fans
    session-verify-user.info
    ndclient.net
    indiaappfest.com
    decloudapp.com
    ...
    ```

    </div>

=== "Curl"

    ``` sh title="related_domains-curl.sh"
    #!/bin/bash

    # Check if a domain is provided
    if [ "$#" -lt 1 ]; then
        echo "Usage: $0 <DOMAIN> [-f|--force]"
        echo "    <DOMAIN> - the domain to query"
        echo "    -f, --force - force downloading all results without prompt, even if results are more than 1000"
        exit 1
    fi

    force_download=false
    if [ "$#" -eq 2 ] && ([[ "$2" == "-f" ]] || [[ "$2" == "--force" ]]); then
        force_download=true
    fi

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"

    # Fetch organization name from WHOIS data
    org_response=$(curl -s -H "X-API-Key: $API_KEY" \
            "$BASE_URL/whois_domains/?q=domain:$1")
    organization=$(echo "$org_response" \
            | jq -r ".items[0].data.registrant.organization")

    if [ "$organization" == "null" ] || [ -z "$organization" ]; then
        echo "No organization found for the domain."
        exit 1
    fi

    # Create query with escaped quotes for download endpoint and URL-encoded query
    domains_query='registrant.organization:"$organization"'
    domains_query_encoded=$(echo -n 'registrant.organization:"$organization"' \
            | jq -Rr @uri)

    #Count domains with the same organization name
    count_response=$(curl -s -H "X-API-Key: $API_KEY" \
            "$BASE_URL/whois_domains_count/?q=$domains_query_encoded")
    count=$(echo "$count_response" | jq ".count")

    # Check if there are any results and optionally ask for confirmation
    if [ "$count" -eq 0 ]; then
        echo "No related domains found."
        exit 1
    elif [ "$count" -gt 1000 ] && ! $force_download; then
        echo "Found $count results for organization: $organization."
        read -p "Do you really want to download all results? (Y/n): " user_input
        if [[ "$user_input" != "Y" && "$user_input" != "y" ]]; then
            echo "Download canceled."
            exit 0
        fi
    fi

    # Fetching domains registered to the same organization
    curl -s -X POST "$BASE_URL/whois_domains/download/" \
            -H "Content-Type: application/json" \
            -H "X-API-Key: $API_KEY" \
            -d '{
                "q": "${domains_query}",
                "fields": ["domain"],
                "source_type": "include",
                "size": "${count}"
                }' | jq -r ".[].data.domain"

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash related_domains-curl.sh microsoft.com 
    Found 33304 results for organization: Microsoft Corporation.
    Do you really want to download all results? (Y/n): Y
    halo5.fans
    session-verify-user.info
    ndclient.net
    indiaappfest.com
    decloudapp.com
    ...
    ```

    </div>

=== "Python"

    ``` py title="related_domains.py"
    import sys
    import netlas
    import json

    # Check if a domain and optionally a force flag are provided
    if len(sys.argv) < 2 or (len(sys.argv) == 3 and sys.argv[2] not in ["-f", "--force"]):
        print("Usage: python related_domains.py <DOMAIN> [-f|--force]")
        print("\t<DOMAIN> - the domain to query")
        print("\t-f, --force - force downloading all results without prompt, even if results are more than 1000")
        sys.exit(1)

    force_download = "-f" in sys.argv or "--force" in sys.argv

    # Initialize the Netlas API assuming that API key is saved
    api_key = netlas.helpers.get_api_key()
    netlas_api = netlas.Netlas(api_key=api_key)

    # Get registrant.organization by domain name
    try:
        org_query = "domain:" + sys.argv[1]
        response = netlas_api.search(query=org_query, datatype="whois-domain")
        organization = response["items"][0]["data"]["registrant"]["organization"]
    except Exception as e:
        print("An error occurred:", str(e))
        sys.exit(2)

    # Fetching domains registered to the same organization
    try:
        # Get the count of domains
        domains_query = 'registrant.organization:" + organization + "'
        cnt_of_res = netlas_api.count(query=domains_query, datatype='whois-domain')

        # Check if there are any results and optionally ask for confirmation
        if cnt_of_res["count"] > 0:
            if cnt_of_res["count"] > 1000 and not force_download:
                print(f"Found {cnt_of_res["count"]} results for organization: {organization}.")
                user_input = input("Do you really want to download all results? (Y/n): ")
                if user_input != "Y":
                    print("Download canceled.")
                    sys.exit(0)

            # Downloading domains
            for resp in netlas_api.download(
                query=domains_query, 
                size=cnt_of_res["count"],
                datatype="whois-domain",
                fields="domain"
            ):
                # decode from binary stream
                response = json.loads(resp.decode("utf-8"))
                print(response["data"]["domain"])
        else:
            print("No results for the given domain.")
    except Exception as e:
        print("An error occurred:", str(e))

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # python related_domains.py microsoft.com 
    Found 33304 results for organization: Microsoft Corporation.
    Do you really want to download all results? (Y/n): Y
    halo5.fans
    session-verify-user.info
    ndclient.net
    indiaappfest.com
    decloudapp.com
    ...
    ```

    </div>

??? note "Please note, data availability depends on your pricing plan"

    The Domain WHOIS Search tool is only available on paid pricing plans, and exceeding the allowed number of results may result in an error.

### Exposed Ports History

This script uses the Netlas Search API to retrieve and display open ports for a specified IP address across indices available to the user, sorted in historical order.

=== "Bash"

    ``` sh title="ip_history-cli.sh"

    #!/bin/bash

    # Check if an IP address is provided
    if [ "$#" -lt 1 ]; then
        echo "Usage: bash ip_history.sh <IP_ADDRESS>"
        exit 1
    fi

    # Define the query
    query="host:$1"

    # Fetch the indices and sort by ID
    indices=$(netlas indices --format json \
        | jq ".[] | select(.type | contains("responses")) | {id, name}" \
        | jq -s "sort_by(.id)")

    # Iterate through indices to fetch exposed ports data
    echo "$indices" | jq -c ".[]" | while read -r idx; do
        index_id=$(echo "$idx" | jq -r ".id")
        index_name=$(echo "$idx" | jq -r ".name")

        # Get count of results
        cnt_of_res=$(netlas count "$query" --indices "$index_id" --format json \
            | jq ".count")

        # Check if there are any results and download them if there are
        if [ "$cnt_of_res" -gt 0 ]; then
            printf "%s\t" "Index #$index_id - $index_name"
            
            responses=$(netlas download "$query" \
                --include "port,protocol" --count "$cnt_of_res" \
                --indices "$index_id" | jq -c ".[]")
            
            exposed_ports=()
            
            while read -r resp; do
                port=$(echo "$resp" | jq -r ".port")
                protocol=$(echo "$resp" | jq -r ".protocol")
                
                # Check for uniqueness and add to array if not already present
                if [[ ! " ${exposed_ports[@]} " =~ " "${port}/${protocol}" " ]]; then
                    exposed_ports+=("${port}/${protocol}")
                fi
            done <<< "$responses" # Pass responses as input to the while loop
            
            # Sort and output data
            sorted_ports=$(printf "%s\n" "${exposed_ports[@]}" | sort -t "/" -k1,1n -k2,2)
             echo $(tr "\n" " " <<< "$sorted_ports")
        fi

        # Wait a second to avoid throttling
        sleep 1
    done

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash ip_history-cli.sh 1.1.1.1
    Index #83 - responses-2022-09-21: 53/dns 80/http 443/https
    Index #84 - responses-2023-01-03: 53/dns 80/http 443/https 2095/http 8080/http
    Index #86 - responses_*_2023-05-11: 53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #89 - responses_*_2023-09-22: 53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #103 - responses_*_2024-01-22:  53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #110 - responses_*_2024-04-25:  53/dns 53/dns_tcp 80/http 443/https 2095/http 2096/https 8080/http 8443/http 8443/https
    ```

    </div>

=== "Curl"

    ``` sh title="ip_history-curl.sh"
    
    #!/bin/bash

    # Check if an IP address is provided
    if [ "$#" -lt 1 ]; then
        echo "Usage: bash ip_history.sh <IP_ADDRESS>"
        exit 1
    fi

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"

    # Define the query
    query="host:$1"

    # Fetch the indices and sort by ID
    indices=$(curl -s "$BASE_URL/indices/" \
        | jq ".[] | select(.type | contains("responses")) | {id, name}" \
        | jq -s "sort_by(.id)")

    # Iterate through indices to fetch exposed ports data
    echo "$indices" | jq -c ".[]" | while read -r idx; do
        index_id=$(echo "$idx" | jq -r ".id")
        index_name=$(echo "$idx" | jq -r ".name")

        # Get count of results
        cnt_of_res=$(curl -s -H "X-API-Key: $API_KEY" \
            "$BASE_URL/responses_count/?q=$query&indices=$index_id" | jq ".count")

        # Check if there are any results and download them if there are
        if [ "$cnt_of_res" -gt 0 ]; then
            printf "%s\t" "Index #$index_id - $index_name"
            
            responses=$(curl -s -X POST "$BASE_URL/responses/download/" \
                -H "X-API-Key: $API_KEY" \
                -H "Content-Type: application/json" \
                -d '{
                    "q": "$query",
                    "fields": ["port","protocol"],
                    "source_type": "include",
                    "size": "${cnt_of_res}",
                    "indices": "$index_id"
                    }')

            exposed_ports=()
            responses=$(echo "$responses" | jq -c ".[]")
            while read -r resp; do
                port=$(echo "$resp" | jq -r ".data.port")
                protocol=$(echo "$resp" | jq -r ".data.protocol")
                
                # Check for uniqueness and add to array if not already present
                if [[ ! " ${exposed_ports[@]} " =~ " "${port}/${protocol}" " ]]; then
                    exposed_ports+=("${port}/${protocol}")
                fi
            done <<< "$responses" # Pass responses as input to the while loop
            
            # Sort and output data
            sorted_ports=$(printf "%s\n" "${exposed_ports[@]}" \
                | sort -t "/" -k1,1n -k2,2)
            echo $(tr "\n" " " <<< "$sorted_ports")
        fi

        # Wait a second to avoid throttling
        sleep 1
    done

    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # bash ip_history-curl.sh 1.1.1.1
    Index #83 - responses-2022-09-21: 53/dns 80/http 443/https
    Index #84 - responses-2023-01-03: 53/dns 80/http 443/https 2095/http 8080/http
    Index #86 - responses_*_2023-05-11: 53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #89 - responses_*_2023-09-22: 53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #103 - responses_*_2024-01-22:  53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #110 - responses_*_2024-04-25:  53/dns 53/dns_tcp 80/http 443/https 2095/http 2096/https 8080/http 8443/http 8443/https
    ```

    </div>

=== "Python"

    ``` py title="ip_history.py"
    import sys
    import time
    import netlas
    import json

    # Check if an IP address is provided
    if len(sys.argv) < 2:
        print("Usage: python ip_history.py <IP_ADDRESS>")
        sys.exit(1)

    # Initialize the Netlas API assuming that API key is saved
    api_key = netlas.helpers.get_api_key()
    netlas_api = netlas.Netlas(api_key=api_key)

    # Define the query
    query = "host:" + sys.argv[1]

    try:
        # Fetch the indices
        indices = netlas_api.indices()

        # Filter indices where the type contains "responses" and sort by id
        filtered_indices = [index for index in indices if "responses" in index["type"]]
        sorted_indices = sorted(filtered_indices, key=lambda x: x["id"])
        
        # Iterating though indices to fetch exposed ports data
        for index in sorted_indices:
            cnt_of_res = netlas_api.count(query=query, indices=str(index["id"]))

            # Check if there are any results and download them if there are
            if cnt_of_res["count"] > 0:
                exposed_ports = []
                print(f"Index #{index["id"]} - {index["name"]}:", end="\t")
                
                for resp in netlas_api.download(
                    query=query, fields="port,protocol",
                    size=cnt_of_res["count"], indices=str(index["id"])
                    ):
                    response = json.loads(resp.decode("utf-8"))

                    # Fill a list with unique ports
                    port = f"{response["data"]["port"]}/{response["data"]["protocol"]}"
                    if port not in exposed_ports:
                        exposed_ports.append(port)

                # Sort and output data
                sorted_ports = sorted(
                    exposed_ports, 
                    key=lambda x: (int(x.split("/")[0]), x.split("/")[1])
                    )
                print(" ".join(sorted_ports))

            # Wait a second to avoid throttling
            time.sleep(1)
    except Exception as e:
        print("An error occurred:", str(e))
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    ~ # python ip_history.py 1.1.1.1
    Index #83 - responses-2022-09-21: 53/dns 80/http 443/https
    Index #84 - responses-2023-01-03: 53/dns 80/http 443/https 2095/http 8080/http
    Index #86 - responses_*_2023-05-11: 53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #89 - responses_*_2023-09-22: 53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #103 - responses_*_2024-01-22:  53/dns 53/dns_tcp 80/http 443/https 2095/http 8080/http
    Index #110 - responses_*_2024-04-25:  53/dns 53/dns_tcp 80/http 443/https 2095/http 2096/https 8080/http 8443/http 8443/https
    ```

    </div>

### See Also

Explore more examples of using the Netlas API in these resources:

- [Netlas Cookbook](https://github.com/netlas-io/netlas-cookbook) contains a lot of automation examples, both using the command line and the Python SDK.
- [Netlas Scripts repo](https://github.com/netlas-io/netlas-scripts) contains automations, which are discussed in detail in our [blog on Medium](https://netlas.medium.com).

<br>
