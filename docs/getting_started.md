---
title: Getting Started
description: Begin your journey with Netlas.io by following our easy start guide. Set up your account, understand tiers, review the features.
---


# Getting Started with Netlas

Welcome to the Netlas platform, which offers a variety of tools aimed at enhancing your internet security research and analysis. This guide will walk you through the initial steps to get started with Netlas, from account creation to exploring the platform's core features.

## Creating an Account

Сreating an account is essential. Registering significantly increases the limits on data requests.

The registration process takes less than a minute. After registration, you will have the Community tier account. This is a forever-free plan. It has some limitations, but still allows you to use most of the Netlas features.


### Third-Party Sign-In

Netlas supports authentication via third-party services like Windows Live and Google. Remember, accounts created this way are linked to the respective service and cannot be converted to a standard account with a separate password.

<center>[Sign up](https://app.netlas.io/registration/){ .md-button .md-button--primary }</center>


## Theme Customization

For a personalized experience, you can switch between :netlas-theme-light: **light** and :netlas-theme-dark: **dark** themes on the top-right corner of the app or in the :material-menu: __Mobile menu__. Select :netlas-theme-system: **system** to match Netlas' theme with your current system preferences.


## Understanding Your Account

Keep an eye on the indicators in the upper right corner. These indicators reflect the limits of your current subscription tier.

<center>
![Netlas coins and requests counters](_assets/coins-requests-counters-image-l.png#only-light){ .bordered }
![Netlas coins and requests counters](_assets/coins-requests-counters-image-d.png#only-dark){ .bordered }
</center>

Search coins are our internal unit of measurement, used to account for the load on the platform. Each time you retrieve a document from the Netlas database, it costs one Search coin.

Another important restriction is the request limit. Like the available amount of Search coins, this limit also depends on your pricing plan.


## Search Tools

Netlas provides a set of powerful [__Search Tools__](https://app.netlas.io/host/). Discover devices, explore data by IP or domain, and utilize filters for precise research.  DNS registry search, WHOIS data search, and SSL certificates search are aslo available.

Try switching between the search tools using buttons on the vertical panel: 
<center>
	:netlas-host:{ .font-large }
	:netlas-responses:{ .font-large }
	:netlas-dns:{ .font-large }
	:netlas-ip-whois:{ .font-large }
	:netlas-domain-whois:{ .font-large }
	:netlas-certificates:{ .font-large }
</center>

Each tool enables you to search through specific data collections. The IP/Domain Info tool provides a summary overview of data, aggregated from all pertinent collections.

Embark on your journey with the :netlas-welcome-tour: __Welcome Tour__, positioned in the bottom left corner. 

Numerous search query examples are readily available for each tool, guiding you through practical applications. To initiate a search, simply click any example. If you want to return from the search results back to the examples page, just click on the icon of the search tool on the left panel.

<center>
![Netlas search tools](_assets/search_tools_examples-l.png#only-light)
![Netlas search tools](_assets/search_tools_examples-d.png#only-dark)
</center>

Netlas Search tools allows you to build complex search queries, download search results, perform facet analysis, and much more.

!!! tip "Featured search queries"

    Hundreds of search queries for the most popular products are published on the :material-github: [__Netlas Dorks__](https://github.com/netlas-io/netlas-dorks) repo.

    Netlas [:fontawesome-brands-x-twitter:](https://twitter.com/Netlas_io) and [:fontawesome-brands-telegram:](https://t.me/netlas) feeds are also good sources of featured query examples.

Learn more about [Search tools &rarr;](search/index.md)

## Discovery Tool

Explore our premier _Attack Surface Discovery Tool_ under the [__Discover__](https://app.netlas.io/asd/) menu item. It's an invaluable resource for reconnaissance, incident investigations, and threat hunting.

Just start with the :netlas-asd-add-node: __Add node__ button. Enter any domain name and click on the node to begin building the attack surface.

<center>
![Netlas Attack Surface Discovery tool](_assets/asd_tool-l.png#only-light)
![Netlas Attack Surface Discovery tool](_assets/asd_tool-d.png#only-dark)
</center>

The tool is designed to find connections between classes of internet entities. For example:

1. If you start with a single domain, you can unveil the registrant company using WHOIS data.

2. The identified company name becomes a conduit to uncover additional domains, registered for the same company.

3. Each domain potentially serves as a pointer to a variety of network entities including, but not limited to, Mail Exchange (MX) servers, Internet Protocol (IP) ranges, Autonomous System Numbers (ASNs), and more.

To truly harness the extensive capabilities of the Attack Surface Discovery tool, unrestricted access to the entirety of Netlas data collections is imperative. The tool works best when it can use a lot of names and contact info.

Learn more about the [Attack Surface Discovery tool &rarr;](easm/discovery.md)

## Private Scanner

Netlas scanners continuously scan the internet across a specific range of ports. As our scanning infrastructure expands – which occurs as the number of subscribers increases – the number of ports scanned also grows. As of mid-2024, this list includes approximately 150 of the most commonly used ports.

The _Netlas Private Scanner_ enable you to overcome this limitation. It available under the [__Scan__](https://app.netlas.io/scanner/) menu item. It allows you to scan any attack surface by checking more than 1,200 of the most common ports. Scanning even thousands of targets takes just a few minutes, and the scan results will be available to you exclusively.

Learn more about the [Private Scanner &rarr;](easm/scanner.md)


## Datastore Access

Though there is no on-premise version of Netlas, datasets are available for download in various formats. Visit the [__Datastore__](https://app.netlas.io/datastore/) to purchase and downloand datasets for internal use.

!!! tip "All datasets are available at no additional cost to Corporate and Enterprise-tier users. [Learn more &rarr;](https://netlas.io/datastore/)"


## Upgrading Your Subscription

We hope this guide helps you get started on your journey with Netlas.

When you decide that the free plan is not enough for your needs and you want to use Netlas to its full potential, you may want to consider purchasing a paid subscription. Pricing plans vary in both data/tools availability and usage limits. 

<center>
![How to change pricing plan](_assets/change_plan-l.png#only-light)
![How to change pricing plan](_assets/change_plan-d.png#only-dark)
</center>

If you represent a company, you have the option to test the app with full-featured access for 2-4 weeks with a [trial subscription](https://netlas.io/sales/). This way, you can see firsthand how it can benefit your company.

<center>
[Compare pricing](https://app.netlas.io/plans/){ .md-button .md-button--primary } 
[Contact sales](https://netlas.io/sales/){ .md-button }
</center>

## Payment Guide

Netlas supports a variety of payment options and a wide range of methods to make accessing our services convenient for everyone.

### One-Time and Recurring Payment Options

We offer two payment options for subscriptions: one-time and recurring. Recurring payments offer automated renewal, which saves time and provides seamless service. One-time payments require manual renewal upon expiration.

!!! tip "We recommend choosing a recurring subscription to avoid the need for manual renewal each month"

For datasets, purchases grant access to the current version only. Each dataset purchase covers a single version, so future updates and new versions would require additional purchases. For continuous access to all updated and future versions of datasets, we recommend the Corporate tier subscription, which provides full access to all datasets in the datastore at no additional cost.

### Fiat Payments

Netlas subscriptions and datasets can be purchased using any major credit card. For self-serve customers, additional payment options, including Apple Pay, Google Pay, and PayPal, are available and will appear at checkout if supported in your country.

For annual subscriptions at the Business tier or higher, we offer the option to [request an invoice](https://netlas.io/sales/), payable via bank transfer.

To securely process fiat payments, we partner with the third-party provider [Paddle](https://paddle.com). Please note that, due to compliance requirements and anti-money laundering regulations, Paddle restricts payments from certain countries. You can review the [list of supported countries here](https://developer.paddle.com/concepts/sell/supported-countries-locales#supported-countries-list).

If the "**Pay in USD**" button is disabled, this indicates that we are unable to process payments in USD from your current location.

### Cryptocurrency Payments

Cryptocurrency payments are accepted; however, please note that recurring payments are not available with this option. You will need to initiate each payment manually. Supported cryptocurrencies include:

- Bitcoin (Lightning available for transactions below $2,500)
- Litecoin
- USDT on the Tron Network

!!! tip "Why choose Lightning for Bitcoin payments?"
    The Bitcoin Lightning Network is an excellent option for transactions due to its speed and low fees.
    
    Many exchanges, such as [Binance](https://www.binance.com), [Kraken](https://www.kraken.com), and [OKX](https://www.okx.com), support Bitcoin Lightning payments.
    
    If you prefer to store your Bitcoin in a wallet, we recommend the super easy-to-use custodial [Wallet of Satoshi](https://www.walletofsatoshi.com) or the open-source, non-custodial [Phoenix Wallet](https://phoenix.acinq.co) — both with Lightning support.

Before proceeding, please ensure that cryptocurrency payments are legal in your country.

<br>
