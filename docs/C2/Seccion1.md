# Elastic Tools

## Introduction

It is a set of tools that creates a data management platform that enables monitoring, consolidating and analysis.

This system is conformed by ElasticSearch, Logstash and Kibana. This set can be recognized as "ELK stack" or just "ELK".

These tools have been created, maintained and distributed by [Elastic](https://www.elastic.co/) since 2012 and have grown according to the market needs. The first tool created was ElasticSearch in 2004 under the name of Compass, but its first official version appeared in 2010.

Elastic offers two kinds of products:
- Open source software, under Apache 2's license, with the exception of some additional features that are distributed with proprietary license. It offers the possibility to check, use and even modify the tools without any cost; but any modifications must be carried out by the user.
- As a paid service, Elastic Cloud makes all the tools and additional features available in servers operated by the team.

## Advantages of ELK platform

ELK offers different solutions to meet the needs in data processing, monitoring and visualization. These solutions can be either paid or free, but what differentiates ELK from others is mainly:

- **Capacity**: It offers many features that can be used in low-cost hardware. Minimum starting settings are needed and many optimizations are available.
- **Scalability**: ElasticSearch is a tool designed to manage terabytes of data. Its architecture enables it to expand quickly and easily.
- **Flexibility**: The settings are flexible and it can adapt to any needs or environment.
- **Access**: Elastic promotes the creation of plugins. A wide array of plugins are available to extend Elastic's functionality and ease of use.
- **Open Source**: Nowadays, open source offers competitive advantages over other platforms as it enables crowdsourced quick error corrections, extension corrections and even the user base extension. It allows the free use of tools, which in turn increases the tool's shared knowledge.

## Components

ELK consists of three essential pillars: ElasticSearch, Logstash and Kibana.

![ELK Platform](elk.png "ELK Platform")

Each component has a specific feature and architecture. Let's see how they are related and how we can use them as a platform.

## Platform Architecture

In this chapter, we will take an in-depth look into how Elastic tools have been used to analyze Mexican Open Contracting data. Before digging into data import, let's see how tools interact.

ELK platform has a linear architecture.

![ELK Stack](elk_stack.jpg "ELK Stack")

1. Data is collected and processed by LogStash
1. LogStash sends ElasticSearch already processed data to be indexed
1. ElasticSearch provides Kibana interface with data to be browsed

Have a closer look at each tool:

- [ElasticSearch](/en/latest/C2/Seccion1/1_ElasticSearch.html)
- [Logstash](/en/latest/C2/Seccion1/2_Logstash.html)
- [Kibana](/en/latest/C2/Seccion1/3_Kibana.html)
