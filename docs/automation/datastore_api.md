---
title: Datastore API
description: Documentation and examples for listing and downloading datasets directly via API or using our Python SDK.
hide:
  - toc
---

# Datastore API

The Netlas Datastore API offers access to a broad range of network-related datasets, published on the [__Netlas Datastore__](https://app.netlas.io/datastore/).

At this moment, the Netlas SDK doesn’t support datastore access. Therefore, all examples below use direct access to the Netlas web-app API.

## Listing Datasets

Anyone can list available datasets by making a GET request to the `/datastore/products/` endpoint. This endpoint does not require authentication. 

```sh
curl -X GET "https://app.netlas.io/datastore/products/"
```
The following example shows how to output available datasets in a human-readable form:

=== "Curl"
    ```sh title="List available datasets"
    curl -s 'https://app.netlas.io/api/datastore/products/' | \
      jq 'sort_by(.data_category.priority, .priority) | .[]' | \
      jq -r '"\(.id)\t\(.datasets[0].added_dt | .[:10])\t\(.pretty_name)"'
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    27  2024-04-22  Known domain names
    23  2024-04-23  Forward DNS (fDNS)
    8   2024-04-23  Forward DNS - A records only
    4   2024-04-21  Forward DNS - MX records only
    69  2024-04-20  Forward DNS - NS records only
    ...
    ```

    </div>

=== "Python"
    ```py title="List available datasets"
    import requests

    # Accessing API endpoint
    response = requests.get('https://app.netlas.io/api/datastore/products/')
    data = response.json()

    # Sorting data by 'priority'
    sorted_data = sorted(data, key=lambda x: (x['data_category']['priority'], x['priority']))

    for product in sorted_data:
        print(f"{product['id']}\t{product['datasets'][0]['added_dt'][:10]}\t{product['pretty_name']}")
    ```

    <div class="result" markdown>

    ``` { .text .no-copy }
    27  2024-04-22  Known domain names
    23  2024-04-23  Forward DNS (fDNS)
    8   2024-04-23  Forward DNS - A records only
    4   2024-04-21  Forward DNS - MX records only
    69  2024-04-20  Forward DNS - NS records only
    ...
    ```

    </div>


## Downloading Datasets

The payment mechanism is solely implemented within the web application; therefore, downloading a dataset via the API is only feasible if it is available to you at no additional cost.

Several datasets are provided free of charge to all Netlas users and can be readily downloaded via the API.

Furthermore, users subscribed to the Corporate and Enterprise pricing plans have access to all datasets without any extra fees. For these users, all datasets are available for download through the API.

=== "Curl"
    ```sh title="Get dataset link"
    #!/bin/bash

    # Define the API key and Dataset ID
    api_key="YOUR_API_KEY"
    dataset_id=28
    url="https://app.netlas.io/api/datastore/get_dataset_link/${dataset_id}/"

    # Make the GET request and format the JSON response
    curl -s -H "X-API-Key: ${api_key}" "${url}" | jq '.'

    ```

    <div class="result" markdown>

    ``` { .json .no-copy }
    [
      {
        "pretty_name": "Known PTR records (rDNS)",
        "name": "known_ptr_records",
        "links": [
          {
            "ext": "json",
            "link": "https://app.netlas.io/storage/known_ptr_records/known_ptr_records.json.zip?md5=MD5Value&expires=1716979209"
          },
          {
            "ext": "csv",
            "link": "https://app.netlas.io/storage/known_ptr_records/known_ptr_records.csv.zip?md5=MD5Value&expires=1716979209"
          }
        ]
      }
    ]
    ```

    </div>

=== "Python"
    ```py title="Get dataset link"
    import requests
    import json

    # Define the API key and Dataset ID
    api_key = 'YOUR_API_KEY'
    dataset_id = 28
    url = f'https://app.netlas.io/api/datastore/get_dataset_link/{dataset_id}/'

    headers = { 'X-API-Key': api_key }

    # Make the GET request and format the JSON response
    response = requests.get(url, headers=headers)
    data = response.json()    
    print(json.dumps(data, indent=4))

    ```

    <div class="result" markdown>

    ``` { .json .no-copy }
    [
      {
        "pretty_name": "Known PTR records (rDNS)",
        "name": "known_ptr_records",
        "links": [
          {
            "ext": "json",
            "link": "https://app.netlas.io/storage/known_ptr_records/known_ptr_records.json.zip?md5=MD5Value&expires=1716979209"
          },
          {
            "ext": "csv",
            "link": "https://app.netlas.io/storage/known_ptr_records/known_ptr_records.csv.zip?md5=MD5Value&expires=1716979209"
          }
        ]
      }
    ]
    ```

    </div>

Most datasets are available in JSON and CSV formats. Dictionaries are supplied in TXT format. Endpoint returns links to all available formats at once.

If you request a bundle, there will be more than one dataset in the JSON list.

<br>