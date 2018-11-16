# Elastic Tools

## Introduction

It is a set of tools that, when combined, creates a data management platform that enables monitoring, consolidating and analyzing them.

This system contains the following tools: ElasticSearch, Logstash and Kibana. This set can be recognized as "stack ELK" or just "ELK". 

These tools have been created, maintained and distributed by [Elastic](https://www.elastic.co/) since 2012 and have grown according to the market needs. The first tool created was ElasticSearch in 2004 under the name of Compass, but its first official version appeared in 2010.

Elastic offers two kinds of products:
- Open source software, under Apache 2's license, with the exception of some additional features that are distributed with proprietary license. It offers the possibility to check, use and even modify the tools without any cost; but any modifications must be carried out by the user.
- As a paid service, Elastic Cloud makes all the tools and additional features available in servers operated by the team.

## Advantages of ELK platform

ELK offers different solutions to meet the needs in data processing, monitoring and visualization. These solutions can be either paid or free, but what differentiates ELK from others is mainly:

- **Capacity**: It offers many features that can be used in low-cost hardware. Minimum starting settings are needed and many optimizations are available. 
- **Scalability**: ElasticSearch is a tool designed to manage TB of data. Its architecture enables it to expand quickly and easily.
- **Flexibility**: The setting is flexible and it can adapt to any needs or environment.
- **Access**: Elastic promotes plugins for its tools with free extra features in order to facilitate the use.
- **Open Source**: Nowadays, open source offers competitive advantages over other platforms as it enables quick error corrections made by the community, extension corrections and even the user base extension. It allows the free use of tools, what increases the tools shared knowledge.

## Components

ELK consists of three essential pillars: ElasticSearch, Logstash and Kibana.

![ELK Platform](elk.png "ELK Platform")

Each component has a specific feature and architecture. Hereunder, we will explain how they are related and how we can use them as a platform.

## Platform Architecture

In this chapter, we will deepen the Elastic tools use to analyze Mexican Open Contracting data. For this, it is necessary to understand better how the different tools are related before we explain how we will import data.

ELK platform has a linear architecture among its components.

![ELK Stack](elk_stack.jpg "ELK Stack")

1. Data is collected and processed by LogStash
1. LogStash sends ElasticSearch already processed data to be indexed
1. ElasticSearch provides Kibana interface with data to be browsed

Have a closer look at each tool:

- [ElasticSearch](https://manualkibanaocds.readthedocs.io/es/latest/C2/Seccion1/1_ElasticSearch.html)
- [Logstash](https://manualkibanaocds.readthedocs.io/es/latest/C2/Seccion1/2_Logstash.html)
- [Kibana](https://manualkibanaocds.readthedocs.io/es/latest/C2/Seccion1/3_Kibana.html)
