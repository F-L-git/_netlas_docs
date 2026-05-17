---
title: Discovery Tool
description: Enhance your cybersecurity with the Attack Surface Discovery Tool — ideal for reconnaissance, security assessments, and threat hunting.
---


# Attack Surface Discovery Tool

You can greatly simplify and improve your work on cybersecurity tasks such as reconnaissance, security assessment, and threat hunting by utilizing the Attack Surface Discovery tool to explore and analyze the relationships between internet entities.

<center>
![Attack surface example](img/intro-l.png#only-light)
![Attack surface example](img/intro-d.png#only-dark)
</center>


The Attack Surface Discovery tool aids in mapping out exposed to the internet parts of any information system, providing a view of potential points of access, including those in third-party and cloud services. It operates with Netlas’s extensive data collections, including internet scanning results, DNS records, WHOIS records, and more.

Hereinafter the tool will be abbreviated as the Discovery tool.


## Discovery Process 

Identifying an attack surface involves mapping out opened-to-the-world points through which data can be entered or extracted. Practically, it means to:

1. Enumerate IP addresses and domains where related data and services can be published.

2. Enumerate and analyze available endpoints.

This process begins with easily discoverable and commonly known parts of an attack surface, such as a domain name or organization name. Access the Discovery tool by clicking on the [__Discover__](https://app.netlas.io/asd/) menu item. Click the :netlas-asd-add-node: __Add node__ button, select the node type, and enter the value. 

<center>
![How to add node to the surface manually](img/add_node-l.png#only-light)
![How to add node to the surface manually](img/add_node-d.png#only-dark)
</center>

!!! tip "Adding and grouping multiple nodes"
    You can easily add a set of objects to the attack surface by using the :netlas-asd-add-nodes: __Add nodes__ button. Simply provide a list of nodes to add, and if you want to group the nodes by their type, select the __Group nodes__ checkbox.

### Relationships

Let's proceed with the key assumption that _parts of a particular attack surface must be related to each other somehow._ Because if they don't, there is no reason to believe that they belong to the same information system. Here are a few examples of such relationships that the Discovery tool helps you find:

- A bunch of domains leads to a single IP or range;
- IP addresses belong to a specific range, the description of which includes the brand name;
- Services published on different hosts provide the same SSL certificate;
- Org name, email addresses, or phone number matches;
- A web server responds with a 301/302 redirect to another domain.

Click on any node on the attack surface to see options for relationship search. The available options depend on the node type. For example, you can search for A-records of a domain and vice versa, search for domains bounded to an IP address.

<center>
![Relationship search options](img/search_options-l.png#only-light)
![Relationship search options](img/search_options-d.png#only-dark)
</center>


Once you have selected your search direction, click the __ADD__ button to make a search and add nodes to the surface. Click on the newly added nodes to continue the relationship discovery process.

If the search returns _more than 20 results_, the __ADD__ button is grayed out. In this case, instead of adding nodes to the surface separately, they can be added as a group by clicking the __ADD GROUP__ button.

??? info "Availability depends on your pricing plan"

    - __The maximum number of nodes per group is limited by your pricing plan.__    
    A search will be unavailable to you if it returns more results than the group capacity limit.

    - __The availability of some search options also depends on data availability restrictions.__    
    For example, if your pricing plan does not provide you with access to contact details such as phone numbers and email addresses, related WHOIS searches will also be unavailable.

    - __Netlas coins and daily request limit are also taken into account.__    
    Regarding this, the Discovery tool is similar to the Netlas Search tools. Searches count towards your daily request limit, each object added to the surface costs 1 coin.

    <center>
    [Compare pricing](https://app.netlas.io/plans/){ .md-button .md-button--primary } &nbsp;
    [Contact sales](https://netlas.io/sales/){ .md-button }
    </center>


### Groups

The groups are very similar to individual nodes. Clicking on a group requests available searches. The group search works the same way as you search every node in the group and join the results.

You can merge nodes and groups in _larger groups_. Just select two or more nodes/groups of the same type with the :netlas-asd-select: __Select nodes__ tool and use the context menu.

The :netlas-asd-ungroup: __Ungroup__ feature is also available in the context menu for groups of 20 nodes or fewer.

Select :material-folder-open: __View list__ from the context menu to see the content of a group. Here you can interact with individual nodes in the group. Pay attention to the :netlas-asd-extract: __Extract__ icon. It allows you to move any node outside the group.

<center>
![Group view](img/group_view-l.png#only-light)
![Group view](img/group_view-d.png#only-dark)
</center>


### Exclusions

If the search returns a node that you don't need to consider as a part of the attack surface, you can :netlas-asd-exclude: __Exclude__ it. Excluded nodes are not searchable. When you download results as a file, they are also excluded.

You can exclude both individual nodes and entire groups. By excluding a group, you are excluding each of its nodes. You can exclude individual nodes inside the group. In this case, a group search will be performed without taking into account excluded nodes.

You can hide excluded nodes by switching the toggle near the zoom control in the bottom-right corner.

### Uniqueness

Every node on the attack surface is unique. It means that you can't add two nodes with the same type and value to the same attack surface.

When a search returns a node that is already on the surface, it simply creates a link. If no nodes are added to the surface after the search, then the same nodes already exist on the surface, either as part of a group or as individual ones.

## Minor Features

The toolbar offers several typical features. :netlas-download: __Download__ and :material-share: __Share__ become available after you save the surface.


<center>
![Discovery toolbar](img/toolbar-l.png#only-light){ .bordered }
![Discovery toolbar](img/toolbar-d.png#only-dark){ .bordered }
</center>

Each time you press the :netlas-save: __Save__ button, the new version of the current attack surface is saved. By pressing the :netlas-open: __Open__ button, you can access to any version saved earlier.

<center>
![Versions of an attack surface](img/surface_versions-l.png#only-light){ .bordered }
![Versions of an attack surface](img/surface_versions-d.png#only-dark){ .bordered }
</center>

You can also :netlas-rename: __Rename__ and :netlas-delete: __Delete__ versions.

The Download tool will return a text file containing domains, IP ranges in the CIDR format, and IP addresses. A file can be sent as input to most network scanners. For example, _nmap_ accepts this format.


## Discovery Strategies

When you build an attack surface, you can move horizontally and vertically. Horizontal search involves finding neighboring entities of the same order. Vertical search involves finding child entities or identifying containing entities.

### Horizontal Relationships

To build a complete attack surface, start by searching for horizontal relationships. The goal is to find as many top-level entities as possible. Try to find alternative domains that belong to the same information system. Search for additional subnets and autonomous systems.

Here are some tips on searching horizontal relationships:

1. MX and NS records are useful in relevant domain searches.
2. Domain WHOIS data is also a way to go.
3. Make use of forward and reverse DNS lookups.
4. IP Whois data gives you additional subnets.
5. Pay attention to redirects and SSL cert lookups.

### Vertical Relationships

Using the Discovery tool, it makes sense to limit vertical relationships search to subdomain enumeration. Searching for published services and endpoints is best done using alternative tools.

When looking for larger parts of an information system, look at the names of autonomous systems and larger networks.

Don't forget to make a forward DNS lookup for a list of subdomains and investigate a group of produced IP addresses. You may decide to return to the horizontal relationships search again starting from these addresses.

!!! tip "Read the [Complete Attack Surface Discovery Guide](https://netlas.io/blog/attack_surface_discovery_guide/) to learn more about basic discovery strategies with practical examples."

## Upcoming Features

We plan to enhance the Discovery tool with additional features in the future. One of the most important features we're working on is __Rebuilding the Surface__. An attack surface stores a history of queries used to build it. In future releases, we will add the ability to monitor changes to the attack surface. Users will receive notifications when modifications are detected in the data for previous searches.

## Hardware Requirements

Handling a large attack surface of over 10,000 nodes and over 100 visible nodes requires powerful hardware. Minimum requirements:

- 4-core 2GHz CPU;
- 8 GB of RAM.


<br>
