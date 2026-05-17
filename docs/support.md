---
title: Support & FAQ
description: Need assistance with Netlas.io? Check out the frequently asked questions and learn how to contact our support team.
hide:
  - toc
---


# Support & FAQ

Please, [contact support](#how-to-contact-support) if you are experiencing technical issues. You can also [connect](#live-community-support) to our Discord Community Server for general questions, discussions, and community support.

A few common questions are described below in the FAQ section:

- [I want to learn search basics through examples. Where I can find some?](#query-examples)
- [How can I download large sets of search results that exceed download limits?](#download-limits)
- [Why am I getting an error when searching by CVE or tag? I have a paid subscription!](#freelancer-cve-search)
- [Downloading is very slow! How can I increase the speed?](#slow-download-speed)
- [How can I search for a specific file in Netlas using its hash or content?](#file-search)


## How to Contact Support

 There is a special in-app feedback form that you can use to ask a technical question, report a bug, or provide feedback.

<center>
![In-app feedback form](_assets/coins-requests-counters-image-l.png#only-light){ .bordered }
![In-app feedback form](_assets/coins-requests-counters-image-d.png#only-dark){ .bordered }
</center>


This form is available under the :netlas-support: __Support & Feedback__ button on the top-right corner of the app and also on the login page in case you are having trouble logging in or creating an account.

<center>
![Login page feedback form](_assets/login-feedback-l.png#only-light){ .bordered }
![Login page feedback form](_assets/login-feedback-d.png#only-dark){ .bordered }
</center>

You can also send an email to support [at] netlas[.]io. But please remember that this method is not recommended.

The time it takes to receive a support response may vary based on your subscription plan. While our standard response time is within one business day, we typically reply much faster.

## Live Community Support

Connect with the Netlas Community on Discord for real-time support and guidance. Engage with fellow users and familiarize yourself with the platform quickly. This is an excellent choice for immediate feedback and community-supported advice. Ideal for general questions or discussions where you can benefit from a variety of perspectives.

<center>[Join Community](https://nt.ls/discord){ .md-button .md-button--primary }</center>

Please note, a Discord account is required to participate. You have to be logged-in to Discord to get connected.


## FAQs

[__Q: I want to learn search basics through examples. Where can I find some?__](#query-examples){ #query-examples }

__A:__ We recommend starting with the [search basics section](search/search_basics.md). It takes only a few minutes to read and provides a solid foundation. For examples of search queries, you can refer to the following resources:

- The [Netlas Dorks Repo](https://github.com/netlas-io/netlas-dorks) contains a comprehensive list of search queries organized by categories such as CVEs, web cameras, routers, and more.
- The [Netlas Cookbook](https://github.com/netlas-io/netlas-cookbook) offers a step-by-step guide to searching and building automation with Netlas.

[__Q: How can I download large sets of search results that exceed download limits?__](#download-limits){ #download-limits }

__A:__ When dealing with large numbers of search results that exceed your download limits, you can increase the limits or segment your queries to manage and download the results in smaller portions.

- __How to increase download limits:__ Download limits vary depending on your pricing plan. Therefore, if you need to download large amounts of data, [upgrading your pricing plan](getting_started.md#upgrading-your-subscription) will be the simplest and most convenient solution.

- __How to segment queries:__ Use additional filters in conjunction with your query to split results into smaller parts. You can split results by geolocation, IP range, or any other filter:
    ```
    <your query> AND geo.continent:("North America" OR "South America")
    <your query> AND geo.continent:("Europe" OR "Asia" OR "Africa" OR "Oceania")
    <your query> AND ip:[1.0.0.0 TO 128.255.255.255]
    <your query> AND ip:[129.0.0.0 TO 255.255.255.255]
    <your query> AND protocol:http
    <your query> AND protocol:https
    ```

[__Q: Why am I getting an error when searching by CVE or tag? I have a paid subscription!__](#freelancer-cve-search){ #freelancer-cve-search }

__A:__ Some Netlas pricing plans include restrictions on the use of certain filters. Search using CVE and tag filters is available starting with the Business pricing plan. Please review the [pricing page](https://app.netlas.io/plans/) to see what search capabilities are available depending on your plan.

[__Q: Downloading is very slow! How can I increase the speed?__](#slow-download-speed){ #slow-download-speed }

__A:__ Downloading search results from Netlas is significantly different from simply transferring a file. When you download search results, the database performs the search on the fly and creates a stream of data. Therefore, the download speed is limited not by the bandwidth of the channel, but by the speed at which the data is retrieved from the index.

Here are some tips to improve the download process:

1. Simplify the search query by eliminating the `*` operator and regular expressions, if possible.
2. When downloading, request only the data you need. For example, you can download only the `uri` field or only IP addresses.
3. Use the Netlas CLI Tool for downloading with the `-o` or `--output_file` option. You will see a preloader indicating the state of the downloading process. Here is an example:
   
   ``` bash
   netlas download tag.name:"laravel" --include uri,ip --all --output_file results.txt
   ```
   <div class="result" markdown>
   ``` { .text .no-copy }
   ⠴ Downloading... ━━━━━━━━━━━━━━━━━━━━━━━━━━━╸━━━━━━━━━━━━  70% 1:47:05  413905/593129
   ```
   </div>

[__Q: How can I search for a specific file in Netlas using its hash or content?__](#file-search){ #file-search }

__A:__ Keep in mind that Netlas focuses on scanning exposed ports. For each service detected, the system makes only one request and saves the response. In the case of the HTTP protocol, it requests an index page. It doesn’t crawl the entire website and its content. So the only option is to search by the file name in the http.body content. If you find links to the file, you can check these links with third-party tools.

```
http.body:"filename"
```
<br>
