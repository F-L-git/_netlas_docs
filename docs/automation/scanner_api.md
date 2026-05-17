---
title: Private Scanner API
description: A comprehensive guide on managing and analyzing network scans using the Netlas Private Scanner API, including endpoints, code examples, and usage tips.
---

# Private Scanner API

The Netlas [Private Scanner](../easm/scanner.md) API provides functionalities for managing and analyzing network scans efficiently.
This includes the ability to start new scans, retrieve past scan data, and check the status of active scans.

**Key Features**

With this API, it is possible to:

- Initiate new scans
- Retrieve a list of previously executed scans
- Check the status of ongoing or completed scans

All scan results are securely stored in a private index, which can be accessed through the [Responses search tool](https://docs.netlas.io/search/responses/).

## Scanner API Endpoints

| HTTP Method | Endpoint                   | Description              |
|-------------|----------------------------|--------------------------|
| `GET`       | /api/scanner/              | List scans               |
| `POST`      | /api/scanner/              | Create scan              |
| `GET`       | /api/scanner/`{id}`/       | Get scan details         |
| `PATCH`     | /api/scanner/`{id}`/       | Update scan details      |
| `POST`      | /api/scanner/change_priority/ | Change scan priority  |
| `DELETE`    | /api/scanner/`{id}`/       | Delete scan              |

### Create Scan

 `POST` /scanner/


To begin a new scan, an API request must include a **set of targets** to be analyzed. The targets define the scope of the scan and determine which network assets will be examined. A [valid list of targets](../easm/scanner.md#supported-target-types) may contain:

- **IP addresses** – Individual IP addresses (e.g., `192.168.1.1`) can be scanned for open ports, services, and vulnerabilities.
- **Domains** – Fully qualified domain names (e.g., `example.com`) allow for DNS-based reconnaissance and analysis.
- **CIDR ranges** – Subnet ranges (e.g., `192.168.1.0/24`) enable scanning of multiple IP addresses within a defined network segment.

Additionally, an optional **label** may be specified to help identify the scan. It can be useful for categorization, filtering, or distinguishing scans conducted for different purposes.

=== "Curl"
    ```bash title="Scan a list of IP addresses and domains"
    curl -X 'POST' "https://app.netlas.io/api/scanner/" \
    -H "X-API-Key: API_KEY" \
    -H "accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{"targets": ["your.target.com", "1.1.1.1", "1.1.1.0/24"], "label": "New Scan"}' | jq "."
    ```

=== "Python"
    ```python title="Scan a list of IP addresses and domains"
    import requests
    import json

    # Set your API key and base URL
    api_key = "your_api_key_here"
    base_url = "https://app.netlas.io/api/scanner/"

    # Define headers for API requests
    headers = {
        "X-API-Key": api_key,
        "accept": "application/json",
        "Content-Type": "application/json"
    }
    # Payload with the targets and label for the scan
    payload = {
        "targets": ["dns.google", "8.8.8.8", "microsoft.com"],
        "label": "New Scan"
    }
    # Send the POST request to create a scan
    response = requests.post(base_url, headers=headers, json=payload)
    data = response.json()  # Parse the JSON response
    print("Create Scan Response:", json.dumps(data, indent=4))
    ```

If any errors occur while creating a scan, refer to the [Error Handling](scanner_api.md#error-handling) section for troubleshooting.

### List Scans and Scan Details

 `GET` /scanner/`{id}`/

The `/scanner/` endpoint provides access to scans associated with the authenticated account. This includes:

- Fetching a list of all available scans
- Retrieving detailed information about a specific scan – By specifying a unique scan ID

To obtain details of an individual scan, use the `/scanner/{id}/` endpoint. Both endpoints return comprehensive information, including scan statuses, labels, and other relevant metadata.

=== "Curl"
    ```bash title="Get list of scans"
    curl -X 'GET' "https://app.netlas.io/api/scanner/" \
    -H "X-API-Key: API_KEY" \
    -H "accept: application/json" | jq "."
    ```

=== "Python"
    ```python title="Get list of scans"
    import requests
    import json

    api_key = "your_api_key_here"
    base_url = "https://app.netlas.io/api/scanner/"

    headers = {
        "X-API-Key": api_key,
        "accept": "application/json",
        "Content-Type": "application/json"
    }
    response = requests.get(base_url, headers=headers)
    data = response.json()  # Parse the JSON response
    print("Scans List:", json.dumps(data, indent=4))  # Pretty print the result
    ```

Call /scanner/`{id}`/ to fetch the same data on particular scan. Both endpoinds provides detailed information, such as scan statuses, labels, and other relevant data.

=== "Curl"
    ```bash title="Get status of particular scan"
    curl -X 'GET' "https://app.netlas.io/api/scanner/SCAN_ID/" \
    -H "X-API-Key: API_KEY" \
    -H "accept: application/json" | jq "."
    ```

    <div class="result" markdown>

    ```{ .json .no-copy title="Abbreviated example of command output" }
    {
    "id": 349,
    "label": "Scan 2024/11/10 21:54:15",
    "targets": [
        "example.com",
        "subdomain.example.com",
        "1.1.1.1",
        "1.1.1.0/24"
    ],
    "scan_started_dt": "2024-11-12T12:31:38Z",
    "scan_ended_dt": "2024-11-12T12:36:40Z",
    "scan_progress": [
        {
        "id": 4284,
        "state": "pending",
        "description": null,
        "timestamp": "2024-11-12T12:31:24.753541Z"
        },
        {
        "id": 4285,
        "state": "scheduled",
        "description": "Scan queued",
        "timestamp": "2024-11-12T12:31:36Z"
        },
        {
        "id": 4286,
        "state": "scanning",
        "description": "1/10 - Scan started",
        "timestamp": "2024-11-12T12:31:38Z"
        },
            . . .
        {
        "id": 4296,
        "state": "done",
        "description": "Scan finished",
        "timestamp": "2024-11-12T12:36:40Z"
        }
    ],
    "created_dt": "2024-11-12T12:31:24.748832Z",
    "index_id": 512,
    "saved_graph": null,
    "estimated_time_to_complete": null,
    "queue_position": null,
    "count": 1826,
    "owner": {},
    "is_scan_from_team": false,
    "status": "done"
    }
    ```

    </div>

=== "Python"
    ```python title="Get status of particular scan"
    import requests
    import json

    api_key = "your_api_key_here"
    base_url = "https://app.netlas.io/api/scanner/"
    scan_id = "required_scan_id"

    headers = {
        "X-API-Key": api_key,
        "accept": "application/json",
        "Content-Type": "application/json"
    }
    # Construct the URL for the specific scan ID
    scan_url = f"{base_url}{scan_id}/"
    # Send the GET request to retrieve the scan details
    response = requests.get(scan_url, headers=headers)
    data = response.json()  # Parse the JSON response
    print("Scan Details:", json.dumps(data, indent=4))
    ```

    <div class="result" markdown>

    ```{ .json .no-copy title="Abbreviated example of command output" }
    {
    "id": 349,
    "label": "Scan 2024/11/10 21:54:15",
    "targets": [
        "example.com",
        "subdomain.example.com",
        "1.1.1.1",
        "1.1.1.0/24"
    ],
    "scan_started_dt": "2024-11-12T12:31:38Z",
    "scan_ended_dt": "2024-11-12T12:36:40Z",
    "scan_progress": [
        {
        "id": 4284,
        "state": "pending",
        "description": null,
        "timestamp": "2024-11-12T12:31:24.753541Z"
        },
        {
        "id": 4285,
        "state": "scheduled",
        "description": "Scan queued",
        "timestamp": "2024-11-12T12:31:36Z"
        },
        {
        "id": 4286,
        "state": "scanning",
        "description": "1/10 - Scan started",
        "timestamp": "2024-11-12T12:31:38Z"
        },
            . . .
        {
        "id": 4296,
        "state": "done",
        "description": "Scan finished",
        "timestamp": "2024-11-12T12:36:40Z"
        }
    ],
    "created_dt": "2024-11-12T12:31:24.748832Z",
    "index_id": 512,
    "saved_graph": null,
    "estimated_time_to_complete": null,
    "queue_position": null,
    "count": 1826,
    "owner": {},
    "is_scan_from_team": false,
    "status": "done"
    }
    ```

    </div>

Key fields in response:

- `id` is unique identifier of scan.
- `label` – scan label assigned by the user (by default it contains creation date and time).
- `targets` – list of targets to scan (IP addresses, domains, subnets).
- `scan_started_dt` (`scan_ended_dt`) is date and time the scan started (completed).
- `scan_progress` is an array with the progress of scan:
  - Another `id` field is unique identifier of progress step.
  - `state` – current status of step (`pending`, `scheduled`, `scanning`, `done`).
- `status` – total scan status (`done`, `failed`, `in_progress`, etc.).
- `count` – total number of results found.

### Update Scan Label

 `PATCH` /scanner/`{id}`/

The `PATCH` method on the `/scanner/{id}/` endpoint enables users to update specific attributes of an existing scan. Currently, the only modifiable attribute is the scan label. This allows users to rename their scans for better organization and clarity.

=== "Curl"
    ```bash title="Modify scan attributes"
    curl -X 'PATCH' "https://app.netlas.io/api/scanner/SCAN_ID/" \
    -H "X-API-Key: API_KEY" \
    -H "accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{"label": "Updated Scan Label"}' | jq "."
    ```

=== "Python"
    ```python title="Modify scan attributes"
    import requests
    import json

    api_key = "your_api_key"
    base_url = "https://app.netlas.io/api/scanner/"
    scan_id = "your_scan_id"
    new_label = "New Scan Updated"

    headers = {
        "X-API-Key": api_key,
        "accept": "application/json",
        "Content-Type": "application/json"
    }
    # URL to update scan details
    update_url = f"{base_url}{scan_id}/"
    payload = {"label": new_label}
    # Send the PATCH request to update the scan
    response = requests.patch(update_url, headers=headers, json=payload)
    data = response.json()  # Parse the JSON response
    print("Updated Scan Response:", json.dumps(data, indent=4))
    ```

### Change Scan Priority

 `POST` /scanner/change_priority/

Allows users to adjust the priority of a scan within the processing queue. This feature is useful for prioritizing urgent scans or delaying less critical ones, ensuring better queue management.

!!! info ""

    The `shift` argument in the `change_priority` request allows you to adjust the **relative position** of a scan in the queue:

    - A **negative** value, e.g., `-2`, moves the scan 2 positions closer **to the front** of the queue, prioritizing it for earlier execution.
    - A **positive** value, e.g., `+3`, moves the scan 3 positions closer **to the back** of the queue, delaying its execution.

    Use negative shift values to speed up the execution of tasks with high priority.

=== "Curl"
    ```bash title="Scan priority alter"
    curl -X 'POST' "https://app.netlas.io/api/scanner/change_priority/" \
    -H "X-API-Key: API_KEY" \
    -H "accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{"id": "SCAN_ID", "shift": SHIFT_POS"}' | jq "."
    ```

=== "Python"
    ```python title="Scan priority alter"
    import requests
    import json

    api_key = "your_api_key"
    base_url = "https://app.netlas.io/api/scanner/"
    scan_id = "required_scan_id"
    shift_pos = "shift_position"

    headers = {
        "X-API-Key": api_key,
        "accept": "application/json",
        "Content-Type": "application/json"
    }
    # URL to change priority
    priority_url = f"{base_url}change_priority/"

    payload = {"id": scan_id, "shift": shift_pos}

    # Send the POST request to change the scan's priority
    response = requests.post(priority_url, headers=headers, json=payload)

    # Check response status
    if response.status_code == 200:
        try:
            data = response.json()  # Parse the JSON response
            print("Change Scan Priority Response:", json.dumps(data, indent=4))
        except json.JSONDecodeError:
            print("Error: Response is not valid JSON.")
            print("Response Text:", response.text)
    else:
        print(f"Failed request: {response.status_code} - {response.text}")
    ```

!!! warning "This adjustment applies only to scans that are not yet scheduled (status `Pending`)."

### Delete Scan

 `DELETE` /scanner/`{id}`/

This endpoint deletes a specific scan by its ID and erases the associated index. Once the scan has been removed, all related data, including the index, will be permanently detached.  

Before deleting, use the [`GET` /scanner/`{id}`/](#list-scans-and-scan-details) method to check the scan data. Make sure that you delete the desired scan, as recovery is only possible for a short period of time.

=== "Curl"
    ```bash title="Delete scan by id"
    curl -X 'DELETE' "https://app.netlas.io/api/scanner/SCAN_ID/" \
    -H "X-API-Key: API_KEY" \
    -H "accept: application/json" | jq "."
    ```

=== "Python"
    ```python title="Delete scan by id"
    import requests
    import json

    api_key = "your_api_key"
    base_url = "https://app.netlas.io/api/scanner/"
    scan_id = "required_scan_id"

    headers = {
        "X-API-Key": api_key,
        "accept": "application/json",
        "Content-Type": "application/json"
    }
    # URL to delete the scan
    delete_url = f"{base_url}{scan_id}/"
    # Send the DELETE request
    response = requests.delete(delete_url, headers=headers)
    if response.status_code == 204:
        print("Scan successfully deleted.")
    else:
        print("Delete Scan Response:", response.json())
    ```

!!! tip "Restore accidentally deleted scans"
    Accidentally deleted scans can be restored if you [contact support](../support.md) within 1 week. <br>
    After 2 weeks, deleted scans are permanently erased and cannot be recovered under any circumstances.

## Error Handling

This section provides an overview of common error responses, their meanings, and recommendations for resolving them.

__Error 400: Max Number of Stored Scans Exceeded__

The number of scans a user can store depends on their subscription plan. When this limit is reached, any attempt to create a new scan will result in this error:

- Delete one or more of your stored scans to [free up space](#delete-scan).  
- Alternatively, consider upgrading your subscription plan if additional storage capacity is required.

__Error 400: Request Parsing Error__

This error occurs when the server cannot process the request due to malformed or invalid input. Common causes include improperly formatted JSON, incorrect parameter names, or missing required fields:

- Verify the request payload and ensure it complies with the API documentation.  
- Check for syntax errors in the JSON data or missing mandatory parameters.

__Error 400: Tool Is Not Available for Current Subscription Plan__

Receiving this error message means that the Scanner is not available in your data plan.
Certain tools or features may not be accessible depending on your subscription tier. If you attempt to use a tool outside the scope of your plan, this error will occur:

- Review the features included in your current subscription plan.  
- Upgrade to a higher-tier plan if the required tool is part of its feature set.

__Error 400: Not Enough Scan Coins__

This error indicates insufficient [__scan coins__](../easm/scanner.md#scan-coins) to perform the requested action.
Scan coins are a consumable resource allocated to your account based on your plan:

- Check your available scan coins on the account dashboard.  
- Re-purchase a subscription to access the Scanner tool availability or wait until the next allocation if your plan includes periodic replenishment.

The number of scan-coins is fixed according to the monthly subscription level.

__Error 403: Current Subscription Plan Does Not Allow Making This Request__

This error occurs when you try to perform an action with the Scan [menu item](https://app.netlas.io/scanner/) or [__API Schema__](https://app.netlas.io/schema/) request, and your data plan limits it. This can happen if you exceed the maximum number of targets you can scan per day, or if you try to store too many scans:

- Confirm the limits and permissions associated with your subscription plan.  
- Upgrade to a plan that provides access to the desired functionality or contact support for assistance.

## Access Scan Results

Scan data can be searched and downloaded using the [Responses Search Tool](../search/responses.md). To access the data, you need to specify the `index_id`, which can be obtained by [listing scans](#list-scans-and-scan-details).

To retrieve the complete list of scan results, use the [Responses Download Endpoint](search_tools_api.md#search-query-api) of the Search Tools API.

=== "Curl"
    ```bash title="Downloading Scan Results"
    #!/bin/bash

    # Define the API key and the base URL for the Netlas API
    API_KEY="YOUR_API_KEY"
    BASE_URL="https://app.netlas.io/api"
    QUERY="host:*.netlas.io"  # Define your search query

    # Get the count of responses for the query
    COUNT=$(curl -s -H "X-API-Key: $API_KEY" \
        "$BASE_URL/responses_count/?q=$QUERY" \
        | jq ".count")

    # Check if responses exist and download them
    if [ "$COUNT" -gt 0 ]; then
        echo "Downloading $COUNT results for query: $QUERY"
        curl -s -X POST "$BASE_URL/responses/download/" \
        -H "Content-Type: application/json" \
        -H "X-API-Key: $API_KEY" \
        -d '{
            "q": "'"${QUERY}"'",
            "fields": ["uri"],
            "source_type": "include",
            "size": '"${COUNT}"'
            }' -o "scan_results.json"
        echo "Results saved to scan_results.json"
    else
        echo "No responses found for the query: $QUERY"
    fi
    ```

=== "Python"
    ```python title="Downloading Scan Results"
    import requests
    import json

    def download_scan_results(api_key, query):
        """
        Downloads scan results from Netlas API using the Download method.

        Parameters:
        - api_key (str): API key for authentication.
        - query (str): Search query to identify results to download.

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
        count_response = requests.get(count_url, headers=headers, params={"q": query, "indices": "your_scan_index_id"})


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
            "fields": ["uri"],  # Adjust fields as needed
            "source_type": "include",
            "size": count,
            "indices": "your_scan_index_id"
        }

        download_response = requests.post(download_url, headers=headers, json=payload)
        if download_response.status_code != 200:
            print(f"Error downloading responses: {download_response.status_code}")
            print("Details:", download_response.json())
            return

        # Save results to file
        file_name = "scan_results.json"
        with open(file_name, "w") as file:
            json.dump(download_response.json(), file, indent=4)
        
        print(f"Scan results saved to {file_name}")

    # Example usage
    API_KEY = "YOUR_API_KEY"
    QUERY = "host:*.netlas.io"

    download_scan_results(API_KEY, QUERY)
    ```

!!! abstract "Access Control"

    - Only the user who initiated the scan has access to their private indexes.
    - If the user is part of a team, all team members can access the private indexes of other participants.
    - Only the owner of a scan can delete a scan and its associated index.

## Example of Usage

Below is an example of a script that:

- Creates a scan using the [Private Scanner](../easm/scanner.md) API.  
- Fetches the list of scans to verify the creation.
- Downloads the results of the scan in JSON format.

=== "Curl"
    ```bash title="netlas_private_scanner.sh"
    #!/bin/bash

    # Usage: ./netlas_private_scanner.sh <api_key> <targets> <output_file>

    API_KEY="$1"
    TARGETS="$2"
    OUTPUT_FILE="${3:-results.json}"

    BASE_URL="https://app.netlas.io/api/scanner/"

    if [ -z "$API_KEY" ] || [ -z "$TARGETS" ]; then
        echo "Usage: $0 <api_key> <targets> [output_file]"
        echo "Example: $0 your_api_key_here \"example.com,8.8.8.8\" results.json"
        exit 1
    fi

    # Step 1: Create a scan
    echo "Creating a new scan..."
    TARGETS_JSON=$(echo "$TARGETS" | jq -R 'split(",")')
    SCAN_RESPONSE=$(curl -s -X POST "$BASE_URL" \
        -H "X-API-Key: $API_KEY" \
        -H "Content-Type: application/json" \
        -d "{\"targets\": $TARGETS_JSON, \"label\": \"Test Scan\"}")

    # Check if the response is an array
    if echo "$SCAN_RESPONSE" | jq -e '.[0]' > /dev/null 2>&1; then
        SCAN_ID=$(echo "$SCAN_RESPONSE" | jq -r '.[0].id')
    else
        SCAN_ID=$(echo "$SCAN_RESPONSE" | jq -r ".id")
    fi

    if [ -z "$SCAN_ID" ] || [ "$SCAN_ID" == "null" ]; then
        echo "Error creating scan: $SCAN_RESPONSE"
        exit 1
    fi
    echo "Scan created with ID: $SCAN_ID"

    # Step 2: Poll for scan status
    while true; do
        STATUS_RESPONSE=$(curl -s -X GET "${BASE_URL}${SCAN_ID}/" \
            -H "X-API-Key: $API_KEY")
        STATUS=$(echo "$STATUS_RESPONSE" | jq -r ".status")

        echo "Current scan status: $STATUS"
        if [ "$STATUS" == "done" ]; then
            echo "Scan completed!"
            break
        elif [ "$STATUS" == "failed" ]; then
            echo "Scan failed: $STATUS_RESPONSE"
            exit 1
        fi
        sleep 30
    done

    # Step 3: Download results
    echo "Downloading scan results..."
    curl -s -X GET "${BASE_URL}${SCAN_ID}/" \
        -H "X-API-Key: $API_KEY" \
        -H "accept: application/json" \
        -H "Content-Type: application/json" \
        -o "$OUTPUT_FILE"

    if [ $? -eq 0 ]; then
        echo "Results saved to $OUTPUT_FILE"
    else
        echo "Error downloading results"
        exit 1
    fi
    ```

=== "Python"
    ```python title="netlas_private_scanner.py"
    #!/usr/bin/env python3

    import requests
    import json
    import argparse
    import time

    BASE_URL = "https://app.netlas.io/api/scanner/"

    def create_scan(api_key, targets, label="New Scan"):
        headers = {
            "X-API-Key": api_key,
            "Content-Type": "application/json"
        }
        payload = {
            "targets": targets,
            "label": label
        }
        response = requests.post(BASE_URL, headers=headers, json=payload)
        if response.status_code == 201:
            scan_data = response.json()
            print("DEBUG: scan_data =", scan_data)  # Debugging output
            if isinstance(scan_data, dict) and "id" in scan_data:
                return scan_data["id"]
            elif isinstance(scan_data, list) and len(scan_data) > 0:
                return scan_data[0].get("id", None)
            else:
                print("Unexpected response format:", scan_data)
                return None
        else:
            print(
                f"Error Creating Scan: {response.status_code} - {response.text}")
            return None

    def poll_scan_status(api_key, scan_id):
        headers = {"X-API-Key": api_key}
        while True:
            response = requests.get(f"{BASE_URL}{scan_id}/", headers=headers)
            if response.status_code == 200:
                status = response.json()["status"]
                print(f"Scan Status: {status}")
                if status == "done":
                    print("Scan completed!")
                    break
                elif status == "failed":
                    print(f"Scan failed: {response.json()}")
                    exit(1)
            else:
                print(
                    f"Error Checking Status: {response.status_code} - {response.text}")
                exit(1)
            time.sleep(30)


    def download_results(api_key, scan_id, output_file):
        headers = {
            "X-API-Key": api_key,
            "accept": "application/json",
            "Content-Type": "application/json"
        }
        response = requests.get(f"{BASE_URL}{scan_id}/", headers=headers)
        if response.status_code == 200:
            try:
                scan_data = response.json()
                with open(output_file, "w") as file:
                    json.dump(scan_data, file, indent=4)
                print(f"Results saved to {output_file}")
            except json.JSONDecodeError:
                print("Error: The response is not valid JSON.")
                print("Server response:", response.text)
        else:
            print(
                f"Error Downloading Results: {response.status_code} - {response.text}")


    if __name__ == "__main__":
        parser = argparse.ArgumentParser(description="Netlas Scanner Script")
        parser.add_argument("api_key", help="API key for authentication")
        parser.add_argument("targets",
                            help="Comma-separated list of targets (e.g., example.com,8.8.8.8)")
        parser.add_argument("-o", "--output",
                            default="results.json", help="Output file for results (default: results.json)")
        parser.add_argument(
            "-l", "--label", default="New Scan", help="Label for the scan")

        args = parser.parse_args()
        targets_list = args.targets.split(",")

        scan_id = create_scan(args.api_key, targets_list, args.label)
        if not scan_id:
            exit(1)

        print("Polling scan status...")
        poll_scan_status(args.api_key, scan_id)

        print("Downloading results...")
        download_results(args.api_key, scan_id, args.output)
    ```

!!! note "Notes in creating scan"
    - Replace `your_api_key_here` with your actual API key.
    - Ensure you have the jq utility installed for parsing JSON in the bash script. Use `sudo apt install jq` to install it on Debain-based systems.

__Usage__

```{ .bash .no-copy }
python3 netlas_private_scanner.py <api_key> <targets> [-o OUTPUT] [-l LABEL]
```

- `<api_key>`: Your Netlas API key (`required`)
- `<targets>`: Comma-separated list of targets to scan (e.g., example.com,8.8.8.8) (`required`)
- `-o, --output`: Specify the output file for saving scan results (`optional`, default: results.json)
- `-l, --label`: Label for the scan (`optional`, default: New Scan)

```bash title="Example of private_scanner.py usage"
python3 netlas_private_scanner.py your_api_key_here \
   "example.com,8.8.8.8,1.1.1.0/24" -o scan_results.json -l "My Custom Scan"
```
