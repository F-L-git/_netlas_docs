---
title: Search Basics
description: Start with the basics of search on Netlas.io. Understand the foundational aspects of building search queries, using filters, and retrieving precise results.
---


# Search Basics

This section contains the most important concepts related to the __Netlas Search__ tools. We recommend that you read the information below carefully if you want to take full advantage of the Netlas Search engine.

## Documents

Netlas stores its data in the form of structured documents. Here is a list of document types for each data collection:

* __Responses search__: each document represents __a scan result__ (server response)   
    and has its unique URI, e.g. `https://app.netlas.io:443/host/`

* __DNS search__: each document represents __a domain__,    
    e.g. `app.netlas.io`

* __IP WHOIS search__: each document represents __an IP range__,     
    e.g. `135.125.236.0 - 135.125.237.255`

* __Domain WHOIS search__: each document represents __a domain__,    
    e.g. `netlas.io`

* __Certificate search__: each document represents __a certificate__,    
    e.g. certificate with MD5 fingerprint `7af6b9b6ac262c3d3dd82d7a44032ba4`

Documents consist of _fields_ and _values_. For example, each response document has `ip`, `port`, and `protocol` fields.

## Search Query Language

Netlas Search tools (except the :netlas-host: __IP/Domain Info__) involve the use of a simple query language, called [Apache Lucene query](https://lucene.apache.org/). It is necessary to understand how to construct search queries in order to use Netlas effectively.

An elementary search query consists of a document field (where to search) and a search phrase (what to search), separated by a colon.

Let's look at the simplest search as an example:

```
ip:1.1.1.1
```

This search query means _"Filter out documents that have the value 1.1.1.1 in the `ip` field"_. This is why fields are also called __filters__ sometimes. 

Some fields contain subfields. They are specified as `field.subfield`. For example, the `geo` field, which is available in the Responses search, contains the `country` subfield.

```
geo.country:US
```

If you want to search for a phrase, use quotes to combine words:

```
http.title:"Mail server"
```

If the search field isn't specified, the search will be performed in the default fields. Each search tool uses its own set of default fields.

Try the following search query in the :netlas-responses: __Responses Search__ tool:

```
"index of"
```

Use an asterisk (`"*"`) operator to search for any value. For example, to search for web pages with any title:

```
http.title:*
```

You can use an asterisk (`"*"`) to address any field or subfield. But in this case, you have to escape it with the back-slash (`""`). The following query will return responses of any protocol with the word "email" in the banner:

```
\*.banner:email
```

If you query an asterisk (`"*"`) without specifying any field, Netlas returns the most recently collected documents from the selected data collection.

### Mapping

All available for searching fields are listed in the mapping. Each data collection has its own mapping. Comments on the purpose of the fields and examples of search queries are given in the following sections of this manual.

Click on any field to put it on the search string.


<center>
![The mapping panel](img/responses_search_mapping-l.png#only-light){ loading=lazy }
![The mapping panel](img/responses_search_mapping-d.png#only-dark){ loading=lazy }
</center>


### Complex Queries

Use `AND`, `OR`, and `NOT` operators (which can also be written as `&&`, `||`, and `!`) to build complex search queries:

```
prot7:ssh AND NOT port:22
```

The default operator is `AND`. It uses when you put two or more search terms separated by space and do not specify any operator between them. The following two queries return the same results:

```
prot7:imap port:993
```
```
prot7:imap && port:993
```

Use brackets to combine search terms:

```
port:(8080 OR 8088 OR 8888) protocol:http
```

```
(ssh:* geo.country:AU) !port:22
```

Note that the `!` operator is not separated by a space.


### Range Search

Specify ranges for date and numeric fields using the `[ TO ]` syntax or `<`, `>`, `=` symbols for one-sided ranges.

```
host:1.1.1.1 port:<=1000
```
```
ip:[173.194.222.0 TO 173.194.222.255]
```

Fields of type _'IP address'_ additionaly support search for a subnet entry:

```
ip:"173.194.222.0/24"
```

Escape the slash if you don't use the quotes. 


### Full-Text Search

Netlas stores text values using two data types: _'TEXT'_ and _'KEYWORD'_. 

The vast majority of fields are of the _'TEXT'_ type. This data type is designed to perform full-text search through large amounts of text data, where the search returns all relevant results rather than just exact matches.

Fields of type _'TEXT'_ store data in the tokenized and normalized form. You can read about the tokenization [here](https://www.elastic.co/guide/en/elasticsearch/reference/current/analysis-overview.html). But essentially, it means that all punctuation, special and service characters are ignored during the search. Search also ignores the difference in letter case, and even some forms of words, e.g. there is no difference between plural and singular.

Let's look at a couple of examples to understand the pros and cons of tokenization:

```
http.title:"index of"
```
Documents with the titles `index of`, `Index of`, `Index of:` `INDEX OF`, `INDEX of`, and even `index /of` will be found.

Another example:

```
http.body:(stephen hawking)
```

Among other results, the search will also return matches containing `Stephen William HAWKING` and documents with links like `http://some-blog.com/label/Stephen%20Hawking/`.

### Exact Match Search

For fields where it makes sense to search only by exact match, the _'KEYWORD'_ type is used. Mostly these are domain names and email addresses. Fields of the _'KEYWORD'_ type don't use tokenization or normalization.

Try the folowing query in the :netlas-dns: __DNS Search__:

```
domain:Netlas.io
```
Most likely you will get nothing, because there is no exact match for `Netlas.io` due to the first capital letter. But the following query should works:
```
domain:netlas.io
```

It is important to understand that the _'KEYWORD'_ type is necessary in some cases. Let's imagine that we used the _'TEXT'_ type for domain names. Then the above search would also return `app.netlas.io`. and even `netlas.io-io-ho.com`.

!!! tip "There is a little trick if you want to search for an exact match in a _'TEXT'_ type field." 

    Try to address it like `field.keyword`:

    ```
    http.title.keyword:"cPanel Login"
    ```

     Most _'TEXT'_ fields have _'KEYWORD'_ subfields for exact match searching. This trick doesn't work with fields longer than 256 characters (like `http.body`) because it's too resource-intensive.


### Fuzzy Search

Use fuzzy querries with ~ operator to search for similar spelling domains and or different forms of words.  You can specify the distanse with number after the fuzzy operator. Supported distances are 1 or 2, default is 2.

```
domain:netlas.io~
```

```
domain:google.com~1
```

### Wildcards and Regex

Finally, you can use wildcards (`"*"` and `"?"`) and regular expressions.

The following query in the :netlas-dns: __DNS Search__ returns google level-2 domains:

```
domain:google.* level:2 
```
  
Regular expression patterns can be embedded in the query string by wrapping them in forward-slashes (`"/"`).

The same query with regex:
```
domain:/google\..*/ level:2
```

Lucene’s regular expression engine does not use the Perl Compatible Regular Expressions (PCRE) library, but it does support [the most of standard operators](https://www.elastic.co/guide/en/elasticsearch/reference/current/regexp-syntax.html).



## Search Tips

It may seem a little confusing at first to write search queries. Try learning from examples:

* Find some examples right in the app under the search bar.

* More examples are available in the :material-github: [__Netlas Dorks__](https://github.com/netlas-io/netlas-dorks) repository.

* Netlas [:fontawesome-brands-x-twitter:](https://twitter.com/Netlas_io) and [:fontawesome-brands-telegram:](https://t.me/netlas) feeds are also good sources of relevant query examples.


### Refining the Query

While searching, use the right panel to refine your query. For example, filter results by geolocation or specific network.

The sidebar can be hidden by clicking the :netlas-sidebar: __Show/Hide sidebar__ button on the toolbar.

<center>
![Netlas Responses Search summary panenl](img/responses_search_summary-l.png#only-light){ .bordered loading=lazy }
![Netlas Responses Search summary pannel](img/responses_search_summary-d.png#only-dark){ .bordered loading=lazy }
</center>

!!! tip "Please note that you can switch between mapping and summary using the tabs at the top of the sidebar."

### Favorites

If you plan to repeat a search after time when new data will be collected, you can save the search query to your favorites. To do this, click on the :material-star-outline: __Add to favorites__ button on the right side of the search bar.

<center>
![Netla search bar](img/search_input-l.png#only-light){ .bordered loading=lazy }
![Netla search bar](img/search_input-d.png#only-dark){ .bordered loading=lazy }
</center>

You can lable and group your favorites.

<center>
![Netla search favorites](img/favorites-l.png#only-light){ .bordered loading=lazy }
![Netla search favorites](img/favorites-d.png#only-dark){ .bordered loading=lazy }
</center>


### View Preferences

Some search tools support view configuration. Click the :fontawesome-solid-gear: __View configuration__ button located on the toolbar to change the settings.

<center>
![Netla search toolbar](img/search_toolbar-l.png#only-light){ loading=lazy }
![Netla search toolbar](img/search_toolbar-d.png#only-dark){ loading=lazy }
</center>


## Historical Search

Netlas collects data in cycles. For example, resolving all domains from the first to the last is a cycle. Each cycle is writed in a separate index. By default, the search is performed in the latest index. Click the :material-calendar: __Index selection__ button on the right side of the search bar to change the search index (see image above).

<center>
![Netla search index selection](img/index_selection-l.png#only-light){ .bordered loading=lazy }
![Netla search index selection](img/index_selection-d.png#only-dark){ .bordered loading=lazy }
</center>

For most data collections, the default index is stored in a high access speed memory (we use large-capacity SSDs). Changing the index significantly affects the search execution time. Therefore, pay attention to the labes:

<code style="background-color:red; color:white; padding: 1px 15px; border-radius: 16px;">fast</code> – The index is stored in a high access speed memory.

<code style="background-color:#64b5f6; color:white; padding: 1px 15px; border-radius: 16px;">slow</code> – The index was moved into memory with a slow access speed.

You may have access to indexes that are currently being updated:

<code style="background-color:#ffa726; color:white; padding: 1px 15px; border-radius: 16px;">indexing</code> – The index is not yet fully built. 

<code style="background-color:#66bb6a; color:white; padding: 1px 15px; border-radius: 16px;">full</code> – The index is fully built.


## Facet Analysis

With facet analysis, you can divide your search results into groups. Any document field can be a grouping criterion.

For example, you can group by the `port` field all responses in the index. This will allow you to find out exactly which ports Netlas scans. To do this, query an asterisk (`"*"`), go to facet analysis by clicking the :netlas-facet: __Group search results__ button on the toolbar and select the `port` field as a grouping criterion.

<center>
![Netla facet analysis](img/facet_analysis-l.png#only-light){ loading=lazy }
![Netla facet analysis](img/facet_analysis-d.png#only-dark){ loading=lazy }
</center>

Facet analysis is quite resource-intensive, so we limit the number of groups in the search results to one thousand.

## Download and Share

Once the search query is built and the search results match what you wanted, you can share the search or download the results.

Find the :netlas-download: __Download results__ button on the toolbar to download search results in JSON or CSV format. It is recommend to select only those fields that you need in the download options. This way you will get results faster.

!!! question "How to download large sets of search results that exceed download limits? [Read the FAQ &rarr;](../support.md#download-limits)"

By clicking on the :material-share: __Share__ button, you can generate a short link and QR-code for the current search query.

<br>

