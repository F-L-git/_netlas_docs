---
title: Discovery Tool API
description: This API provides a structured way to discover and analyze nodes within your attack surface, enabling users to identify vulnerabilities and improve their security posture.
---

# Attack Surface Discovery Tool API

Article provides an overview of the Attack Surface Discovery Tool API, detailing its endpoints, parameters, and expected responses. The API allows users to discover and analyze nodes within their attack surface.

## Discovery API Endpoints

| HTTP Method | Endpoint                                 | Description                                                  |
|-------------|------------------------------------------|--------------------------------------------------------------|
| `POST`      | /discovery/group_of_nodes_count/         | Returns documents containing count and preview for searches. |
| `POST`      | /discovery/group_of_nodes_result/        | Returns nodes found by the given search query.               |
| `POST`      | /discovery/node_count/                   | Returns documents containing count and preview for searches. |
| `POST`      | /discovery/node_result/                  | Returns nodes found by the given search query.               |

## Endpoint Details

### Overview of Grouped Nodes

`POST` /discovery/group_of_nodes_count/

Used to retrieve the count of documents that match specified search criteria along with a preview of those documents. 
It is particularly useful for understanding how many results are available for given targets before performing more detailed queries.

__Parameters__

- `data`: object (body)
    * `node_type` (string): Type of node (e.g., IP, domain, etc.). This parameter specifies the kind of entity you are querying.
    * `node_value` (array of strings): List of scan target nodes. This is where you provide the actual targets you want to analyze.

=== "Curl"
    ```bash title="Count Matching Nodes Across Groups"
    curl -X 'POST' \
    'https://app.netlas.io/api/discovery/group_of_nodes_count/' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -H "X-API-Key: API_KEY" \
    -d '{
    "node_type": "example_node_type",
    "node_value": [
        "example_node_value1",
        "example_node_value2" ] }'
    ```

=== "Python"
    ```python title="Count Matching Nodes Across Groups"
    import requests

    def group_of_nodes_count(api_key, node_type, node_values):
        """
        Sends a POST request to the /group_of_nodes_count/ endpoint.
        
        Parameters:
        - api_key (str): Your API key for authentication.
        - node_type (str): Type of the node.
        - node_values (list): List of scan target nodes.

        Returns:
        - JSON response from the API.
        """
        url = "https://app.netlas.io/api/discovery/group_of_nodes_count/"
        headers = {
            "accept": "application/json",
            "Content-Type": "application/json",
            "X-API-Key": api_key
        }
        data = {
            "node_type": node_type,
            "node_value": node_values
        }
        response = requests.post(url, headers=headers, json=data)
        return response.json()
    ```

### Grouped Nodes Report

`POST` /discovery/group_of_nodes_result/

Retrieves nodes found based on the provided search query.
This endpoint is useful for asset discovery by aggregating related nodes based on specific search criteria.

__Parameters__

- `data`: object (body)
    * `node_type` (string): Type of node.
    * `node_value` (array of strings): List of scan target nodes.
    * `search_field_id` (string): Search field identifier that specifies which results to retrieve.

=== "Curl"
    ```bash title="Retrieve Nodes Matching a Query"
    curl -X 'POST' \
    'https://app.netlas.io/api/discovery/group_of_nodes_result/' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -H "X-API-Key: API_KEY" \
    -d '{
    "node_type": "example_node_type",
    "node_value": [
        "example_node_value1" ],
    "search_field_id": "example_search_field_id" }'
    ```

=== "Python"
    ```python title="Retrieve Nodes Matching a Query"
    def group_of_nodes_result(api_key, node_type, node_values, search_field_id):
        """
        Sends a POST request to the /group_of_nodes_result/ endpoint.
        
        Parameters:
        - api_key (str): Your API key for authentication.
        - node_type (str): Type of the node.
        - node_values (list): List of scan target nodes.
        - search_field_id (str): Search field identifier.

        Returns:
        - JSON response from the API.
        """
        url = "https://app.netlas.io/api/discovery/group_of_nodes_result/"
        headers = {
            "accept": "application/json",
            "Content-Type": "application/json",
            "X-API-Key": api_key
        }
        data = {
            "node_type": node_type,
            "node_value": node_values,
            "search_field_id": search_field_id
        }
        response = requests.post(url, headers=headers, json=data)
        return response.json()
    ```

### Total Nodes Summary

`POST` /discovery/node_count/

Provides an overview of available data related to a specific node. It returns information such as the total number of matching documents, a preview of relevant records, and the name of the search field used to retrieve the data.

__Parameters__

- `data` : object (body)
    * `node_type` (string): Type of node.
    * `node_value` (string): Scan target nodeyou want to analyze.

=== "Curl"
    ```bash title="Node Search Overview with Counts"
    curl -X 'POST' \
    'https://app.netlas.io/api/discovery/node_count/' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -H "X-API-Key: API_KEY" \
    -d '{
    "node_type": "example_node_type",
    "node_value": "example_node_value" }'
    ```

=== "Python"
    ```python title="Node Search Overview with Counts"
    def node_count(api_key, node_type, node_value):
        """
        Sends a POST request to the /node_count/ endpoint.
        
        Parameters:
        - api_key (str): Your API key for authentication.
        - node_type (str): Type of the node.
        - node_value (str): Scan target node.

        Returns:
        - JSON response from the API.
        """
        url = "https://app.netlas.io/api/discovery/node_count/"
        headers = {
            "accept": "application/json",
            "Content-Type": "application/json",
            "X-API-Key": api_key
        }
        data = {
            "node_type": node_type,
            "node_value": node_value
        }
        response = requests.post(url, headers=headers, json=data)
        return response.json()
    ```

__Example API Request and Response for Netlas Discovery Tool__

This example demonstrates how to use the Discovery API to retrieve information about a specified node (such as a domain) and analyze its associated data. The API request allows to explore domain-related records, including TXT records, mail servers, NS servers, and subdomains.

=== "Curl"
    ```bash title="Example Curl Request"
        curl -X 'POST' \
        'https://app.netlas.io/api/discovery/node_count/' \
        -H 'accept: application/json' \
        -H 'Content-Type: application/json' \
        -H 'X-API-Key: YOUR_API_KEY' \
        -d '{
            "node_type": "domain",
            "node_value": "google.com"
        }'
    ```    

    ??? abstract "Node Discovery API Results Overview"
        <div class="result" markdown>

        ```json title="Response for Discovery Node Count"
            [
            {
                "aggregations": [
                {
                    "search_field": "TXT records for domain",
                    "count": 12,
                    "preview": [
                    "apple-domain-verification=30afibcvsudv2plx",
                    "cisco-ci-domain-verification=479146de172eb01ddee38b1a455ab9e8bb51542ddd7f1fa298557dfa7b22d963",
                    "docusign=05958488-4752-4ef2-95eb-aa7ba8a3bd0e",
                    "docusign=1b0a6754-49b1-4db5-8540-d2c12664b289",
                    "facebook-domain-verification=22rm551cu4k0ab0bxsw536tlds4h95"
                    ],
                    "search_field_id": 32,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Mailservers for domain",
                    "count": 1,
                    "preview": [
                    "smtp.google.com"
                    ],
                    "search_field_id": 31,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "NS servers for domain",
                    "count": 4,
                    "preview": [
                    "ns1.google.com",
                    "ns2.google.com",
                    "ns3.google.com",
                    "ns4.google.com"
                    ],
                    "search_field_id": 30,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "A records for domain",
                    "count": 818,
                    "preview": [
                    "13.248.243.5",
                    "23.227.38.65",
                    "23.227.38.74",
                    "60.191.3.139",
                    "64.233.161.100"
                    ],
                    "search_field_id": 29,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "For whom it is mailserver",
                    "count": 569,
                    "preview": [
                    "111creative.com",
                    "121dd.com",
                    "21.sipka.org",
                    "22.sipka.org",
                    "221.sipka.org"
                    ],
                    "search_field_id": 33,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "For whom it is nameserver",
                    "count": 38,
                    "preview": [
                    "allgodsdaughtersministry.org",
                    "artrake.com",
                    "asmallcompany.net",
                    "beautifullywicked.org",
                    "cafejo.cl"
                    ],
                    "search_field_id": 34,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "Subdomains",
                    "count": 667080,
                    "preview": [
                    "-he3mkxy6qpnsolajdulboj27aij2shy2tfjek34fo4sy7xp47qnq.mx-verification.google.com",
                    "-tuw4angritg6ht0pslzdjyjznectbjqpcb4pimfgvc.mx-verification.google.com",
                    "0-2qy0n_vgzxrjb1gjiqw9hwes-hnen-42qrozws8r4.mx-verification.google.com"
                    ],
                    "search_field_id": 35,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "Registrant phone from WHOIS",
                    "count": 0,
                    "preview": [],
                    "search_field_id": 39,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Registrant email from WHOIS",
                    "count": 1,
                    "preview": [
                    "select request email form at https://domains.markmonitor.com/whois/google.com"
                    ],
                    "search_field_id": 38,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Organization from WHOIS",
                    "count": 1,
                    "preview": [
                    "Google LLC"
                    ],
                    "search_field_id": 36,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Registrant person from WHOIS",
                    "count": 0,
                    "preview": [],
                    "search_field_id": 37,
                    "is_too_much_docs": false
                }
                ]
            }
            ]
        ```

        </div>

=== "Python"
    ```python title=""
    import requests

    def fetch_node_count(api_key, node_type, node_value):
        """
        Sends a POST request to the Netlas /discovery/node_count/ endpoint.

        Parameters:
        - api_key (str): Your API key for authentication.
        - node_type (str): The type of node (e.g., 'domain').
        - node_value (str): The value of the node to analyze (e.g., 'google.com').

        Returns:
        - dict: The JSON response from the API.
        """
        url = "https://app.netlas.io/api/discovery/node_count/"
        headers = {
            "accept": "application/json",
            "Content-Type": "application/json",
            "X-API-Key": api_key
        }
        payload = {
            "node_type": node_type,
            "node_value": node_value
        }

        response = requests.post(url, headers=headers, json=payload)
        if response.status_code == 200:
            return response.json()
        else:
            response.raise_for_status()

    # Example usage
    api_key = "YOUR_API_KEY"
    node_type = "domain"
    node_value = "google.com"

    try:
        response_data = fetch_node_count(api_key, node_type, node_value)
        print("Response:", response_data)
    except requests.exceptions.RequestException as e:
        print("An error occurred:", e)
    ```

    ??? abstract "Node Discovery API Results Overview"
        <div class="result" markdown>

        ```json title="Response for Discovery Node Count"
            [
            {
                "aggregations": [
                {
                    "search_field": "TXT records for domain",
                    "count": 12,
                    "preview": [
                    "apple-domain-verification=30afibcvsudv2plx",
                    "cisco-ci-domain-verification=479146de172eb01ddee38b1a455ab9e8bb51542ddd7f1fa298557dfa7b22d963",
                    "docusign=05958488-4752-4ef2-95eb-aa7ba8a3bd0e",
                    "docusign=1b0a6754-49b1-4db5-8540-d2c12664b289",
                    "facebook-domain-verification=22rm551cu4k0ab0bxsw536tlds4h95"
                    ],
                    "search_field_id": 32,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Mailservers for domain",
                    "count": 1,
                    "preview": [
                    "smtp.google.com"
                    ],
                    "search_field_id": 31,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "NS servers for domain",
                    "count": 4,
                    "preview": [
                    "ns1.google.com",
                    "ns2.google.com",
                    "ns3.google.com",
                    "ns4.google.com"
                    ],
                    "search_field_id": 30,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "A records for domain",
                    "count": 818,
                    "preview": [
                    "13.248.243.5",
                    "23.227.38.65",
                    "23.227.38.74",
                    "60.191.3.139",
                    "64.233.161.100"
                    ],
                    "search_field_id": 29,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "For whom it is mailserver",
                    "count": 569,
                    "preview": [
                    "111creative.com",
                    "121dd.com",
                    "21.sipka.org",
                    "22.sipka.org",
                    "221.sipka.org"
                    ],
                    "search_field_id": 33,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "For whom it is nameserver",
                    "count": 38,
                    "preview": [
                    "allgodsdaughtersministry.org",
                    "artrake.com",
                    "asmallcompany.net",
                    "beautifullywicked.org",
                    "cafejo.cl"
                    ],
                    "search_field_id": 34,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "Subdomains",
                    "count": 667080,
                    "preview": [
                    "-he3mkxy6qpnsolajdulboj27aij2shy2tfjek34fo4sy7xp47qnq.mx-verification.google.com",
                    "-tuw4angritg6ht0pslzdjyjznectbjqpcb4pimfgvc.mx-verification.google.com",
                    "0-2qy0n_vgzxrjb1gjiqw9hwes-hnen-42qrozws8r4.mx-verification.google.com"
                    ],
                    "search_field_id": 35,
                    "is_too_much_docs": false
                }
                ]
            },
            {
                "aggregations": [
                {
                    "search_field": "Registrant phone from WHOIS",
                    "count": 0,
                    "preview": [],
                    "search_field_id": 39,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Registrant email from WHOIS",
                    "count": 1,
                    "preview": [
                    "select request email form at https://domains.markmonitor.com/whois/google.com"
                    ],
                    "search_field_id": 38,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Organization from WHOIS",
                    "count": 1,
                    "preview": [
                    "Google LLC"
                    ],
                    "search_field_id": 36,
                    "is_too_much_docs": false
                },
                {
                    "search_field": "Registrant person from WHOIS",
                    "count": 0,
                    "preview": [],
                    "search_field_id": 37,
                    "is_too_much_docs": false
                }
                ]
            }
            ]
        ```

        </div>

### Node Analysis

`POST` /discovery/node_result/

The `/discovery/node_result/` endpoint allows users to retrieve nodes that match a specific search query. This can be used to analyze and extract structured information about targeted nodes based on predefined search parameters.

__Parameters__

- `data`: object (body)
    * `node_type` (string): Type of node.
    * `node_value` (array of strings): List of scan target nodes.
    * `search_field_id` (string): Search field identifier that specifies which results to retrieve.

=== "Curl"
    ```bash title="Fetch Nodes by Search Field"
    curl -X 'POST' \
    'https://app.netlas.io/api/discovery/node_result/' \
    -H 'accept: application/json' \
    -H 'Content-Type: application/json' \
    -H "X-API-Key: API_KEY" \
    -d '{
    "node_type": "example_node_type",
    "node_value": [
        "example_node_value1" ],
    "search_field_id": "example_search_field_id" }'
    ```

=== "Python"
    ```python title="Fetch Nodes by Search Field"
    def node_result(api_key, node_type, node_values, search_field_id):
        """
        Sends a POST request to the /node_result/ endpoint.
        
        Parameters:
        - api_key (str): Your API key for authentication.
        - node_type (str): Type of the node.
        - node_values (list): List of scan target nodes.
        - search_field_id (str): Search field identifier.

        Returns:
        - JSON response from the API.
        """
        url = f"https://app.netlas.io/api/discovery/node_result/"
        
    headers = {
        "accept": "application/json",
        "Content-Type": "application/json",
        "X-API-Key": api_key
    }
    data = {
        "node_type": node_type,
        "node_value": node_values,
        "search_field_id": search_field_id
    }
    response = requests.post(url, headers=headers, json=data)
    return response.json()
    ```

## Error Handling

This section provides an overview of common error responses, their meanings, and recommendations for resolving them.

__Responses__

When using the API, it is crucial to handle errors appropriately:

- **400 Bad Request**: Required parameters missing or data validation error. Ensure all required parameters are included in your requests to avoid this error.
- **402 Payment Required**: Not enough coins to perform the query. Monitor your coin balance before making requests.

!!! info "Check your available [__scan coins__](../easm/scanner.md#scan-coins) on the account dashboard."

- **403 Forbidden**: Action not allowed; details included in response. Check your permissions and ensure you have access rights for the requested action.
- **429 Too Many Requests**: Daily request limit exceeded or request throttled. Be aware of your daily request limits and adjust your usage accordingly.

When using the API, it is crucial to handle errors appropriately:

- Ensure all required parameters are included in your requests to avoid `400` errors.
- Monitor your coin balance to prevent `402` errors.
- Be aware of your daily request limits to avoid `429` errors.

__Error 400: Missing Required Parameters__

This error occurs when required parameters are missing from your request, or if the data submitted doesn't meet validation rules:

- Ensure all mandatory fields are included in your request.
- Double-check for any required parameters that may have been omitted, as outlined in the API documentation.

__Error 400: Invalid Data Format__

This error arises when the data in your request is not formatted correctly, such as improperly structured JSON, invalid data types, or incorrect parameter values:

- Verify the format and structure of your request payload.
- Ensure that the data types and values match what is specified in the API documentation.

__Error 402: Insufficient Coins__

If your coin balance is too low to perform the requested action, you will receive this error. Your account may not have enough coins to complete the query:

- Check your available scan coins on the account dashboard.
- Top up your coin balance by purchasing more coins if needed.

__Error 403: Action Not Allowed__

This error occurs when the requested action is not allowed based on your account permissions, such as attempting to access a restricted tool or feature:

- Verify your permissions and ensure that the requested action is authorized under your account.
- Check your subscription plan to ensure that you have access to the requested tools or features.

__Error 429: Rate Limit Exceeded__

If you've hit the daily request limit or are being throttled due to too many requests, you will encounter this error:

- Monitor your request rate and adjust your usage to stay within your daily limits.
- Wait for the cooldown period to pass before attempting additional requests.

By adhering to the best practices outlined above, you can effectively handle common errors when interacting with the API, ensuring smooth usage of the tool. 

For detailed information on error handling, refer to the [Error Handling](scanner_api.md#error-handling) section of the Scanner API documentation.

## Access Scan Results 

To access the scan results in the Discovery Tool API, you need to use the `search_field_id` obtained from the `/discovery/group_of_nodes_count/` endpoint. After obtaining the search field ID, you can query the results for the discovered nodes.
To download the results, you can use the following example scripts.

=== "Curl"
    ```bash title="Query results for discovered nodes"
    #!/bin/bash

    # Define API key and base URL
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"
    QUERY="node_type:example_node_type AND node_value:example_node_value"  # Define your query

    # Step 1: Get the count of matching responses
    COUNT=$(curl -s -H "X-API-Key: $API_KEY" \
        "$BASE_URL/responses_count/?q=$QUERY" \
        | jq ".count")

    # Step 2: Download results if there are any
    if [ "$COUNT" -gt 0 ]; then
        echo "Downloading $COUNT results for query: $QUERY"
        curl -s -X POST "$BASE_URL/responses/download/" \
            -H "Content-Type: application/json" \
            -H "X-API-Key: $API_KEY" \
            -d '{
                "q": "'"${QUERY}"'",
                "fields": ["uri", "ip", "port"],  # Adjust as needed
                "source_type": "include",
                "size": '"${COUNT}"'
            }' -o "discovery_scan_results.json"

        echo "Results saved to discovery_scan_results.json"
    else
        echo "No responses found for the query: $QUERY"
    fi
    ```

=== "Python"
    ```python title="Query results for discovered nodes"
    import requests
    import json

    def download_discovery_results(api_key, query):
        """
        Downloads discovery scan results using the responses/download endpoint.

        Parameters:
        - api_key (str): API key for authentication.
        - query (str): Search query combining node_type and node_value.

        Returns:
        - None. Saves results to a JSON file.
        """
        base_url = "https://app.netlas.io/api"
        headers = {
            "X-API-Key": api_key,
            "accept": "application/json",
            "Content-Type": "application/json"
        }

        # Step 1: Get the response count
        count_url = f"{base_url}/responses_count/"
        count_response = requests.get(count_url, headers=headers, params={"q": query})

        if count_response.status_code != 200:
            print(f"Error fetching response count: {count_response.status_code}")
            print("Details:", count_response.json())
            return

        count = count_response.json().get("count", 0)
        if count == 0:
            print(f"No responses found for query: {query}")
            return

        print(f"Found {count} responses for query: {query}")

        # Step 2: Download the responses
        download_url = f"{base_url}/responses/download/"
        payload = {
            "q": query,
            "fields": ["uri", "ip", "port"],  # Adjust fields as needed
            "source_type": "include",
            "size": count
        }

        download_response = requests.post(download_url, headers=headers, json=payload)
        if download_response.status_code != 200:
            print(f"Error downloading responses: {download_response.status_code}")
            print("Details:", download_response.json())
            return

        # Save results to a file
        file_name = "discovery_scan_results.json"
        with open(file_name, "w") as file:
            json.dump(download_response.json(), file, indent=4)

        print(f"Discovery scan results saved to {file_name}")

    # Example usage
    if __name__ == "__main__":
        API_KEY = "YOUR_API_KEY"
        NODE_TYPE = "example_node_type"  # Replace with actual node type
        NODE_VALUES = ["example_node_value1", "example_node_value2"]  # Example values
        QUERY = f"node_type:{NODE_TYPE} AND node_value:{' '.join(NODE_VALUES)}"

        download_discovery_results(API_KEY, QUERY)
    ```

!!! note "Explanation of key points"
    - **Query Composition**: Both scripts dynamically build a query to include `node_type` and `node_value` for filtering results.
    - **Download Results**: The `/responses/download/` endpoint fetches the results, saving them in JSON format.
    - **Fields Filtering**: You can include specific fields (e.g., uri, ip, port) using the fields parameter with `source_type` set to `"include"`.
    - **Output File**: The results are saved into a JSON file called `discovery_scan_results.json`. Adjust the filename if needed.

## Usage Exapmle

Here’s an example script that demonstrates how to use the defined functions to interact with the [Discovery Tool](../easm/discovery.md) API. This script will perform a search for available scanning nodes and download the results.

=== "Curl"
    ```bash title="netlas_discovery.sh"
    #!/bin/bash

    # Set your API key here
    API_KEY="<your_api_key_here>"

    # Define the base URL for the Netlas API
    BASE_URL="https://app.netlas.io/api/discovery/"

    # Example parameters for testing
    NODE_TYPE="<example_node_type>"  # Replace with actual node type (e.g., 'ip', 'domain')
    NODE_VALUES=("example_node_value1" "example_node_value2")  # Replace with actual values

    # Function to send a POST request to the /discovery/group_of_nodes_count/ endpoint
    function group_of_nodes_count() {
        local node_type=$1
        local node_values=$2
        curl -X POST \
            "$BASE_URL/group_of_nodes_count/" \
            -H "accept: application/json" \
            -H "Content-Type: application/json" \
            -H "X-API-Key: $API_KEY" \
            -d '{
                    "node_type": "'"$node_type"'",
                    "node_value": ['"$(IFS=, ; echo "${node_values[*]}")"']
                }'
    }

    # Function to send a POST request to the /discovery/group_of_nodes_result/ endpoint
    function group_of_nodes_result() {
        local node_type=$1
        local node_values=$2
        local search_field_id=$3
        curl -X POST \
            "$BASE_URL/group_of_nodes_result/" \
            -H "accept: application/json" \
            -H "Content-Type: application/json" \
            -H "X-API-Key: $API_KEY" \
            -d '{
                    "node_type": "'"$node_type"'",
                    "node_value": ['"$(IFS=, ; echo "${node_values[*]}")"'],
                    "search_field_id": "'"$search_field_id"'"
                }'
    }

    # Function to save the result to a JSON file using jq
    function save_results_to_file() {
        local response=$1
        local filename=$2
        echo "$response" | jq '.' > "$filename"
    }

    # Example usage
    echo "Step 1: Get count of nodes"
    count_response=$(group_of_nodes_count "$NODE_TYPE" "${NODE_VALUES[@]}")
    echo "Group of Nodes Count: $count_response"

    # Step 2: Extract search field ID from the count response (assuming it returns a valid structure)
    search_field_id=$(echo "$count_response" | jq -r '.search_field_id')

    if [ "$search_field_id" != "null" ]; then
        # Step 3: Get nodes result based on the search field ID
        echo "Step 3: Get nodes result"
        result_response=$(group_of_nodes_result "$NODE_TYPE" "${NODE_VALUES[@]}" "$search_field_id")
        echo "Group of Nodes Result: $result_response"

        # Step 4: Save results to a file
        save_results_to_file "$result_response" "scan_results.json"
        echo "Results saved to scan_results.json"
    else
        echo "No valid search field ID found in count response."
    fi
    ```

=== "Python"
    ```python title="netlas_discovery.py"
    import requests
    import json

    # Set your API key here
    API_KEY = "<your_api_key_here>"

    # Define the base URL for the Netlas API
    BASE_URL = "https://app.netlas.io/api/discovery/"

    # Headers for the requests
    HEADERS = {
        "accept": "application/json",
        "Content-Type": "application/json",
        "X-API-Key": API_KEY
    }

    # Function to send POST requests to the /discovery/group_of_nodes_count/ endpoint
    def group_of_nodes_count(node_type, node_values):
        endpoint = f"{BASE_URL}group_of_nodes_count/"
        data = {
            "node_type": node_type,
            "node_value": node_values
        }
        response = requests.post(endpoint, headers=HEADERS, json=data)
        return response.json()

    # Function to send POST requests to the /discovery/group_of_nodes_result/ endpoint
    def group_of_nodes_result(node_type, node_values, search_field_id):
        endpoint = f"{BASE_URL}group_of_nodes_result/"
        data = {
            "node_type": node_type,
            "node_value": node_values,
            "search_field_id": search_field_id
        }
        response = requests.post(endpoint, headers=HEADERS, json=data)
        return response.json()

    # Function to save results to a JSON file
    def save_results_to_file(results, filename):
        with open(filename, 'w') as f:
            json.dump(results, f, indent=4)

    # Example usage
    if __name__ == "__main__":
        # Example parameters for testing
        node_type = "<example_node_type>"  # Replace with actual node type (e.g., 'ip', 'domain')
        node_values = ["<example_node_value1>", "<example_node_value2>"]  # Replace with actual values

        # Step 1: Get count of nodes
        count_response = group_of_nodes_count(node_type, node_values)
        print("Group of Nodes Count:", count_response)

        # Step 2: Extract search field ID from count response (assuming it returns a valid structure)
        if count_response and 'search_field_id' in count_response:
            search_field_id = count_response['search_field_id']  # Adjust according to actual response structure

            # Step 3: Get nodes result based on the search field ID
            result_response = group_of_nodes_result(node_type, node_values, search_field_id)
            print("Group of Nodes Result:", result_response)

            # Step 4: Save results to a file
            save_results_to_file(result_response, 'scan_results.json')
            print("Results saved to scan_results.json")
        else:
            print("No valid search field ID found in count response.")
    ```

Explanation of the Script:

- `API Key` and `Base URL`: The script starts by defining your API key and the base URL for the Netlas API.
- Headers: It sets up headers that will be used in all requests to specify that we are expecting JSON responses and include the API key for authentication.
- Functions:
    * `group_of_nodes_count`: Sends a request to get the count of nodes based on specified parameters.
    * `group_of_nodes_result`: Sends a request to retrieve detailed results based on the search field ID.
    * `save_results_to_file`: Saves the retrieved results into a JSON file for later analysis.

__Usage__

It defines example parameters such as `node_type` and `node_values` (these should be replaced with actual values).
It calls `group_of_nodes_count` to get the count of nodes.
If successful, it extracts the `search_field_id` from the response and uses it to call `group_of_nodes_result`.
Finally, it saves the results into a JSON file named `scan_results.json`.

!!! tip ""
    Make sure to replace placeholder values such as `your_api_key_here`, `example_node_type`, and others with actual values when implementing your solution."

__Usage Command Line Example__

Here’s how you can run this script from the command line:

```{ .bash .no-copy }
python3 netlas_discovery.py <api_key> <targets> [-o OUTPUT] [-l LABEL]
```
Arguments:

- `<api_key>`: Your Netlas API key (required)
- `<targets>`: Comma-separated list of targets to scan (e.g., example.com,8.8.8.8) (required)
- `-o, --output`: Specify the output file for saving scan results (optional, default: results.json)
- `-l, --label`: Label for the scan (optional, default: New Scan)

Example Command:
```bash
python3 netlas_discovery.py your_api_key_here \
   "example.com,8.8.8.8,1.1.1.0/24" -o scan_results.json -l "My Custom Scan"
```

This command runs the scanner with your specified targets and saves the output in a file named scan_results.json. Adjust parameters as needed for your specific use case.

## Related Articles

For a comprehensive understanding of the Netlas platform and its APIs, refer to the following articles:

- **[Private Scanner API](../automation/scanner_api.md)**

    Describes how to configure and use the private scanning feature, which allows you to run scans on isolated or private networks securely.

- **[Search Tools API](../automation/search_tools_api.md)**

    Explains how to use the Search Tools API for querying and analyzing search results, as well as integrating search functionality into your security operations. Essential for leveraging the search capabilities of the platform.

- **[__API Schema__](https://app.netlas.io/schema/)**

    Offers detailed information about the available fields, data types, and structures used within the Discovery and Scanner tools. Essential for building complex queries and analyzing scan results.

Each article covers the purpose, endpoints, and usage scenarios specific to the respective API. Use these resources to build a robust security monitoring and analysis system.
