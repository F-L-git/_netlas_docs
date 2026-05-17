---
title: Changelog
description: Explore the latest updates, enhancements, and fixes on the Netlas platform. Stay informed with our Changelog for all product and feature developments.
---

# Changelog

<!--
## Netlas Python SDK v.0.7.0

*Published on 2025-00-00*

The Netlas CLI tool is now available via Homebrew for easy installation on macOS and Linux. [Learn more](https://github.com/netlas-io/homebrew-netlas).

**Improved:**
- Improved error handling for API responses, including Cloudflare-related errors.
- Added support for additional HTTP error codes, such as 524 (timeout), 521 (server temporarily down), and 429 (rate limiting).
- Implemented automatic throttling for 429 Too Many Requests errors, ensuring the SDK waits for the server-specified retry time before resending requests.
-->

## Netlas v.1.0.9

*Published on 2024-12-27* 🎄🎁✨

This release focuses on improving fault tolerance, refining key features, and fixing several bugs to enhance the overall user experience.

**Improved:**

- Enhanced fault tolerance to ensure continuous performance even when some database servers are unavailable.
- Optimized the autocomplete feature.
- Added a link to the status page in the footer and on the maintenance page.
- Various minor improvements to the user interface.

**Fixed:**

- Resolved issues related to exceptional cases with subscriptions, such as when the payment infrastructure provider cannot process recurring payments.
- Corrected the calculation of the estimated time to complete (ETC) in the Private Scanner.
- Fixed an issue preventing the deletion of saved attack surface versions and frozen scans.
- Addressed a bug where refreshing the page while working with a saved attack surface in Discovery no longer requires saving it as a new file instead of a version of an existing surface.

------

## Netlas Python SDK v.0.6.1

*Published on 2024-12-20*

This update adds support for the [Netlas Datastore API](../automation/datastore_api.md), enabling users to list available datasets and retrieve download links directly via SDK and CLI.

**Added:**

- Retrieve available dataset names and metadata using CLI command `netlas datastore list` or SDK method `Netlas.datasets()`.
- Obtain direct download links for selected datasets via CLI command `netlas datastore get <id>` netlas and SDK method `Netlas.get_dataset_link(id)`.

**Improved:**

- Improved CLI usability and `--help` option.

------


## Netlas v.1.0.3

*Published on 2024-11-19*

This release focuses on improving the payment experience, and updating core components for improved performance and security.

**Improved:**

- A [Payment Guide](../getting_started.md#payment-guide) link has been added to the payment form, making it easier to navigate through the payment process.
- A new payment notice now highlights important Freelancer subscription plan terms directly on the payment page for better clarity.
- The _"Pay in USD"_ button is disabled for countries unsupported by our payment provider.

**Fixed:**

- Our custom _"Not Found"_ page now correctly returns a 404 status code, aligning with standard web practices.
- Small adjustments have been made to autocomplete functionality, improving its accuracy. 

**Core Updates:**

- Many internal components and various frameworks we use have been updated to their latest versions, enhancing system performance and stability.

------

## :fireworks: Netlas v.1.0.0

*Published on 2024-10-28*

This milestone release brings enhanced payment flexibility to our users. Now users can choose between two billing options: one-time payments, which offer access for a specific period without automatic renewal, and recurring payments, which automatically renew subscriptions at the end of each billing cycle, ensuring continuous access without the need for manual renewal.

**Added:**

- Recurrent payment support for all pricing plans available via the web app.

**Fixed:**

- Netlas Discord Server link fixed.

------


## Netlas v.0.25.1

*Published on 2024-10-03*

This minor update focuses on enhancing the functionality and user experience of the Netlas platform by improving the integration of the private scanner, optimizing the private scanner’s scheduling algorithm, and fixing some key issues.

**Improved:**

- IP/Domain info is now displays data from your private scans when it's more recent than public scan data.
- A Search Tools UI improved – the search string remains visible at the top of the screen even as you scroll.
- The scheduling algorithm for private scans has been improved.
- Various minor enhancements and adjustments.

**Fixed:**

- Resolved an issue that caused incomplete data collection when scanning smaller networks.
  
------

## Netlas Private Scanner (v.0.25.0)

*Published on 2024-09-16*

We are thrilled to announce a major update to the Netlas App, featuring the highly anticipated **Netlas Private Scanner**. This new tool allows users to perform fast, passive scans across more than 1,200 ports, making it ideal for efficiently scanning large attack surfaces. Alongside this, we've also introduced **Team Access**, enabling users to collaborate and share attack surfaces and scans within their teams.

**Added: Netlas Private Scanner**

- Perform passive scans of IPs, domains, or CIDRs with more than 1,200 ports covered.
- Supports the scanning of arbitrary lists of targets or pre-built attack surfaces.
- Seamlessly integrate with existing discovery tools for automated scanning of attack surfaces.
- Prioritize and manage multiple scans with the drag-and-drop queue system.
- Easily view scan results and apply advanced filters using the integrated Responses Search Tool.

**Added: Team Access**

- Collaborate with your teammates by sharing attack surfaces and scans within your team.
- Team members gain read-only access to view and share scans and attack surfaces created by other team members.

**Improved:**

- Host method now shows the latest scan data, including data from private scans.
- Pricing updated with new features.
- Various minor enhancements and adjustments.

**Fixed:**

- Discovery Download method now ignores excluded IPs instead of subtracting them from IP ranges.

## Netlas v.0.24.1

*Published on 2024-07-31*

This release brings small but notable improvements to the Attack Surface Discovery Tool. The most significant enhancement is the ability to add multiple nodes to the surface simultaneously. We have also introduced a system theme property, allowing your system to control the dark/light color scheme on the Netlas app and website. This is particularly convenient if your system changes colors based on the time of day.

**Added:**

- Batch addition of nodes to the attack surface.
- Drag-and-drop support for grouping.
- System theme property, enabling your system preferences to control the dark/light theme.

**Improved:**

- Various minor enhancements and adjustments.
- Subscription option for blog updates now available in the app.

------

## Netlas v.0.24.0

*Published on 2024-05-24*

This update marks the beginning of our partnership with [RST Cloud](https://www.rstcloud.com), bringing advanced threat intelligence data directly within the Netlas [IP/Domain Info Tool](../search/host.md). This integration empowers Netlas users with deeper insights into the risk and reputation of IP addresses and domains.

**Added:**

- The Netlas IP/Domain Info Tool now displays threat intelligence data.

**Improved:**

- __Important__: The API endpoint `/api/datastore/get_dataset_link/{product_id}` has been updated. The JSON structure returned by this endpoint has been changed to make it more convenient and easier to use.
- The pricing table has been slightly restructured for better readability, making it easier to understand our pricing options.
- Various minor UI improvements and adjustments.

**Fixed:**

- A bug related to the searching by favicon feature has been fixed.

------

## Netlas Python SDK v.0.5

*Published on 2024-05-17*

Due to compatibility issues with the `orjson` package across different platforms, we have replaced it with the standard Python `json` package. This change ensures a smoother installation of the Netlas Python SDK on any platform.

**Added:**

- New CLI command option `--all`: Use `netlas download --all <query>` to request a count and download all available results for a specific query.
- Added the corresponding `Netlas.download_all(query)` method in the Python SDK.

**Improved:**

- Replaced the `orjson` package with the standard `json` package to enhance cross-platform compatibility.
- Enhanced the `download` command by adding a progress bar when used with the `-o` or `--output_file` options.

------


## Netlas v.0.23.4

*Published on 2024-05-06*

**Added:**

- Facet analysis mode has been added to the IP WHOIS and Domain WHOIS search tools.

**Improved:**

- The full-screen map view in the Responses search tool now includes an editable header and QR code.
- The Share button is now available in facet analysis mode.
- __Important__: Web-app URLs for the IP WHOIS and Domain WHOIS search tools have been changed from `/whois/ip/` to `/whois_ip/`, and from `/whois/domains/` to `/whois_domains/`.
- Various minor UI improvements.

**Fixed:**

- The OpenStreetMap copyright notice has been restored to the `host` view.
- An error when grouping by IP ranges in facet analysis mode.

------


## Netlas v.0.23.3

*Published on 2024-04-18*

This minor update follows our recent penetration testing, which identified several vulnerabilities in our subscription system. While these issues were not critical, they were significant enough to require immediate attention.

**Added:**

- Option to request community support via our Discord server.

**Improved:**

- Enhanced the structure and informativeness of page titles.
- User interface improvements for Favorites and the mobile menu.

**Fixed:**

- A bug in the password reset form.
- An issue with duplicating tags.

**Security:**

- Resolved one major and two minor vulnerabilities in the subscription system discovered during the penetration test.


------


## Netlas v.0.23.1

*Published on 2024-04-08*

**Improved:**

- New API endpoints for performing facet analysis (grouping search results) have been published: `responses_facet`, and `domains_facet`.
- __Important__: The `stat` endpoint, which was previously used for grouping, has been deprecated and will soon be removed. If you used it in API queries, replace the corresponding calls with one of the new facet analysis methods.
- Third-party sign-in feature now warns if you previously registered using a password and vice versa.
- API schema documentation (Swagger UI) has been significantly improved. Added descriptions of methods and possible error codes.
- Detailed instructions for using Netlas search tools have been published on the documentation portal.
- The maximum number of requests per day for unregistered users has been reduced from 30 to 10.
- Some minor improvements and optimizations.

**Fixed:**

- An annoying bug in autocomplete that occasionally deleted part of the search query entered by the user.
- Search field in the tags window is now works correctly.

------
 


## Major Update of Attack Surface Discovery Tool (v.0.23.0)

*Published on 2024-03-21*

Groups are now supported. With this new feature, building large surfaces up to 1,000,000 nodes is now easy. Usability has been greatly improved with group search and other minor features.

**Added to Discovery tool:**

- Groups, group operations and group searches support.
- The Searches and Nodes panel replaced with the History panel.
- Actions are logged in details to the history panel.
- The color scheme improved for surface elements.
- Excluded nodes can be hidden.
- Feature availability depends on the pricing plan.

**Improved:**

- The pricing table extended with the Discovery options, some options are reordered.
- Top menu redesigned.

**Fixed:**

- Minor bugs.

------




## Attack Surface Discovery Tool (v.0.22.0)

*Published on 2023-07-14*

In this big update a new tool was added - Attack Surface Discovery tool. Now you can conveniently build your surfaces using Netlas data and visualize them as graphs. At the moment, the tool works with restrictions that will be removed later. We hope you enjoy it and look forward to your feedback!

**Added:**

- Attack Surface Discovery tool.

**Improved:**

- Switching between summary and mapping in sidebar.
- When the limit is exceeded, Netlas API returns error 429 (previously 420).

**Fixed:**

- Checking the availability of payment services.
- WHOIS options in autcomplete.

------



## Netlas v.0.21.0

*Published on 2023-03-15*

This new release brings a long awaited feature – *Bookmarks.* Now you can save your favorite search queries by clicking on the star icon in the search string.

Search by favicon feature is significantly improved. Now can search not only exact matches, but also nearest matches. We use perceptual hash for this. Perceptual hash algorithms are opposite to standard cryptographic hashes — they are optimized to change as little as possible for similar inputs. So you can find answers with favicons that look pretty close to a given input, but use a different color, for example.

**Added:**

- Favorites – saved search queries.
- JARM fingerprinting for HTTPs protocols.

**Improved:**

- Index IDs are now in order.
- Domain whois collection have some improvements.

**Fixed:**

- Some minor improvements.

------

## Netlas Python SDK v.0.4.0

*Published on 2022-12-07*

**Added:**

- Whois-ip and whois-domain datatypes in `download` method.
- Whois-ip and whois-domain datatypes in `count` method.

**Changed:**

- Rename `query` to `search`.

------


## Netlas Python SDK v.0.3.0

*Published on 2022-11-11*

**Added:**

- Include/exclude fields for `query`, `download` and `host`
- Whois-domain search.

**Changed:**

- Migrate whois searches in query function.
- Download to stdout by default.
- `-s` server flag removed.

------



## Netlas Beta (v.0.18.0)

*Published on 2022-10-10*

With this build, Netlas.io goes to the Beta testing phase. 

We added new search tools - host summary and domain whois search, new protocols, privacy detection features and much more. But the general novation is a subscription system. 

Check out our pricing.#nbsp;*We'll kick off with an 80% discount sale!*#nbsp;Discounts will decrease every few months as the service improves.

**Added:**

- Subscription system and pricing.
- Domain whois search tool.
- Host view (IP and domain summary).
- Privacy detection on the host view - VPN/Tor/Proxy.
- Organization to which an IP is delegated on the host view.
- DNS protocol (both TCP and UDP) protocol support.
- Modbus protocol support.
- Siemens S7 communications protocol support.

**Improved:**

- Profile page redesigned, a list of orders added.
- Search tooltips redesigned.

**Fixed:**

- Some minor improvements.



------



## Netlas v.0.16.0

*Published on 2022-05-18*

The first Netlas.io app release was published on the 19th of May in 2021. A year ago. Today we publish a new "Birthday" release with many new outstanding features! A new data collection has been added, the set of supported protocols has been expanded, search examples and help pages have been redesigned and many more features. Details are listed below.

**Added:**

- IP whois data collection with a dedicated search interface.
- UDP protocol support.
- AMQP (Advanced Message Queuing Protocol) support.
- MQTT (Message Queue Telemetry Transport Protocol) support.
- SNMP (Simple Network Management Protocol) support.
- NTP (Network Time Protocol) support.
- SOCKS protocol support.
- NetBIOS support.
- Netlas.io Datastore now supports crypto payments.

**Improved:**

- search examples redesigned and followed with help articles.
- HTTP mapping changed (favicon and on-page contacts are subfields of the http field now).
- whois mapping is changed (the asn field is now a subfield of the whois field).

**Fixed:**

- Some minor improvements.

------



## Netlas v.0.14.0

*Published on 2021-12-13*

Did you know, Netlas.io operates more than 1,7 billion domain names? All types of DNS records are available. Improved datastore and new datasets come with the last Netlas.io update.

**Added:**

- New datasets and dataset bundles.
- Datasets purchasing is now available for unregistered users (except free datasets).
- New indices with fresh scan results (responses, DNS, certificates indices).

**Improved:**

- The mapping component now consists of categories and has better scrolling.
- Hide/show summary button.
- Improved error messages.

**Fixed:**

- Autofocus in search and mapping.
- Some minor improvements.

------



## Netlas v.0.13.0

*Published on 2021-10-13*

Netlas.io Datastore is here! Now you can download Netlas.io data in CSV and JSON. We have already prepared several datasets. Please, write us what datasets you want to see in Netlas.io Datastore.

**Added:**

- Netlas.io Datastore.
- Search hints.
- Indicies page in the Help section.
- Search by Tag tool in the Responses search.

**Improved:**

- When clicking on mapping fields, the search query is expanding, not replacing.
- Reset API key button requires confirmation (pop-up window added).
- Now you can copy API key with a button on the profile page.

**Fixed:**

- Some minor UI improvements.

------



##  Netlas v.0.12.0

*Published on 2021-09-03*

Wow!!! Just take a look at these awesome charts and map we've added to the Netlas.io web app.

**Added:**

- Statistics mode allows you to group search results (press chart button on the top of the search results page).
- Beautiful fullscreen summary map in the responses view (press the full-screen button to see it).
- PTR or domain name (if available) near IP address on the top of each response.
- Domains tab in the responses view shows related to the IP-address domain names.
- Contacts tab in the responses view shows contacts founded on the web page.
- State and speed indicators in the index selection menu.
- Now you can hide and show columns in the table view (press the gear button on the top of search results of domains and certs views).
- 'source_type' and 'fields' parameters allowing to select fields to search.
- Summary pane in the domains view.
- Ability to specify return fields in `host` API endpoint.

**Improved:**

- Smart mapping filter on the mapping pane.
- Autocomplete suggestions.
- Changes in url scheme (from `/` to `/responses/`).
- API endpoint `host` lost `q` param: `/api/host/<ip or domain>/`.
- Empty request to API endpoint `host` returns client's IP info.

**Fixed:**

- CVE marking bug.
- Path-related links after renames.
- Duplicates in downloads bug.
- Search history ordering.
- Field organization renamed to Issuer in certificates view.

------


## Netlas Python SDK v.0.1.6

*Published on 2021-08-05*

**Added:**

- New command `stat` - generate data statistics.

**Improved:**

- Interraction with the `host` API.

------



##  Netlas v.0.11.0

*Published on 2021-07-28*

Not so much added this time. There was a lot of backend related tasks. Also we are about to release a first version of Netlas Stats – a tool for making charts based on Netlas data aggregations.

**Added:**

- Index selection on Certificates search page.
- Summary panel on Domain search page.
- CNAME record on Domain search page.

**Improved:**

- API & SDK host method.

**Fixed:**

- Summary panel on Responses search page available again.
- Large file downloading error.
- Escaping commas in csv download method.

**Temporary unavailable:**

- Host page under redesign right now.

------


## Netlas Python SDK v.0.1.4

*Published on 2021-06-18*

**Added:**

- Pagination in query search.
- Sphinx documentation configs.
- Specify data index for search, count, etc.
- Function and cli-command to retrieve available data indexes.

**Improved:**

- Raw stream downloading.
- Migrate API key from GET parameter to HEADER.
- Colored APIError exceptions.

**Fixed:**

- Downloading via python lib.
- Downloading by given query.
- Updated profile endpoint.
- Commit fix.
- Bump2version config auto commit.

------



## Netlas v.0.10.24

*Published on 2021-06-10*

We've just published a minor Netlas update. There are small improvements and bug fixes. There was also a critical incident when upgrading to a newer version of our DBMS. We apologise for any inconvenience caused by the service interruption.

**Added:**

- Links to Host view added to responses search results.
- User can specify file name and choose fields when downloading search results.
- Index selection added to Domains search view.

**Improved:**

- Host page redesigned.

**Fixed:**

- 'No results found' message added for empty search query.
- Recaptcha changed to Invisible reCAPTCHA v2.
- A number of small UI bugs fixed.

**Temporary unavailable:**

- Stats panel on the Responses search page (we are working on returning it back).

------



## Netlas Alpha (v.0.10.9)

*Published on 2021-05-19*

We are proud to present **Netlas.io alpha release**!

Don’t judge strictly. We've only just begun. There are a lot of cool features to implement in the near future. But we already have data to share right now. So here are our first release notes.

**Signin/Signup:**

- Netlas account.
- Signup through Google, Microsoft.

**Responses search:**

- Search and filter responses using query language.
- Search in the specific scan.
- Search examples.
- Search query autocomplete.
- Download data in JSON format.
- Search by image (favicon).
- Description, response, certificate, whois and cve tabs in results.
- The Statistics window.

**DNS search:**

- Search and filter DNS records using query language.
- A, NS, MX and TXT DNS records.
- Search examples.
- Search query autocomplete.
- Download data in JSON format.

**Certificates search:**

- Search and filter certificates using query language.
- Search examples.
- Search query autocomplete.
- Download data in JSON format.

**Host Page:**

- Get aggregated information about IP: geo, ports, protocols, tags, vulnerabilities, domains, referrers.
- Get aggregated information about domain name: geo, referrals, ip, paths.

**Help Page:**

- Welcome tour.
- A brief description of the pages and their elements, as well as a small guide to use.
- Query syntax.
- Data collections.
- Description of collections with examples of queries that can be immediately opened by clicking.
- Scanners, scans and datasources.
- API and SDK documentation.

**Feedback Window:**

- Sending feedback with the ability to attach attachments.

**Profile Page:**

- User information.
- Change the api key.
- Generate the QR code of the api key.
- Refresh coins (temporarily).
- Hint what Netlas Coin is.
- Displays the current amount of Netlas Coins.
- Displays how many Netlas Coins have been spent in total.
- Information about the achievements.
- Search history.

------

## :rocket: Launch

*Project launched on 2020-11-06*  