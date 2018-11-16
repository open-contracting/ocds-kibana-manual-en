## An Introduction to the Open Contracting Data Standard

> *This introduction has been taken from the chapter [An introduction to the Open Contracting Data Standard](https://github.com/rparrapy/ocds-r-manual/blob/master/manual.Rmd#an-introduction-to-the-open-contracting-data-standard) of the Manual [Analyzing Open Contracting data using the R programming language](https://github.com/rparrapy/ocds-r-manual/blob/master/manual.Rmd) by [Rodrigo Parra](https://github.com/rparrapy)*

By requiring data-sharing in a structured, re-usable and machine readable form; Open data opens up new opportunities for analysis and citizen engagement and participation. The [Open Contracting Data Standard](https://www.open-contracting.org/data-standard/) was created to apply these principles to the complete contracting life-cycle; including planning, tender, award, contract and implementation.

The data standard, designed and developed through an open process by [Open Contracting Partnership (OCP)](https://www.open-contracting.org/?lang=es), allows governments and cities around the world to share their contracting data, enabling greater transparency in public contracting, and supporting accessible and in-depth analysis of the efficiency, effectiveness, fairness, and integrity of public contracting systems. Additionally, the help desk team, staffed by Open Data Services Co-operative, is available to assist prospective users in their journey towards adoption of the standard. 

The intention of this section is to introduce the reader to the standard, the use cases it was designed for and the basic concepts needed to apply it. Most of the content was taken from the official documentation of the standard; for a more thorough introductory walktrough, please refer to the [OCP getting started guide](http://standard.open-contracting.org/latest/en/getting_started/).

### Users and use cases

The standard was designed with four main groups of user needs:

* Achieving value for money for government
* Strengthening the transparency, accountability and integrity of public contract
* Enabling the private sector to fairly compete for public contracts
* Monitoring the effectiveness of service delivery

To find out about who is using OCDS-compliant data around the globe and how they are doing it, have a look at the [OCP] website(http://www.open-contracting.org/). Four potential use cases for open contracting data are:

* Value for money in procurement: helping officials get good value for money during the procurement process, and analyzing whether these goals were achieved afterwards.
* Detecting fraud and corruption: identifying red flags that might indicate corruption by studying individual procurements or networks based on funding, ownership and interests.
* Competing for public contracts: allowing private firms to understand the potential pipeline of procurement opportunities by looking at information related to past and current procurements.
* Monitoring Service Delivery: helping interested actors to leverage traceability in the procurement process for monitoring purposes, linking budgets and donor data to the contracts and results.

### The contracting process

The standard defines a contracting process as:

> All the planning, tendering information, awards, contracts and contract implementation information related to a single initiation process.

The standard covers all the stages of a contracting process, even though some processes might not involve all possible steps. For identification purposes, all contracting processes are assigned a unique Open Contracting ID (ocid), which can be used to join data from different stages. In order to avoid ocid clashes between publishers, a publisher can prepend a prefix to locally generated identifiers. Publishers are encouraged to register their prefix [here](http://standard.open-contracting.org/latest/en/implementation/registration/).



### Documents

Contracting processes are represented as **documents** in OCDS. Each document is made up of several **sections**, mentioned below:

* **Release metadata**: contextual information about each release of data;
* **Parties**: information about the organizations and other participants involved in the contracting process;
* **Planning**: information about the goals, budgets and projects a contracting process relates to;
* **Tender**: information about how a tender will take place, or has taken place;
* **Awards**: information on awards made as part of a contracting process;
* **Contract**: information on contracts signed as part of a contracting process;
* **Implementation**: information on the progress of each contract towards completion.

An example JSON snippet compliant with this structure looks as follows:

```{json}
{
   "language": "en",
   "ocid": "contracting-process-identifier",
   "id": "release-id",
   "date": "ISO-date",
   "tag": ["tag-from-codelist"],
   "initiationType": "tender",
   "parties": {},
   "buyer": {},
   "planning": {},
   "tender": {},
   "awards": [ {} ],
   "contracts":[ {
       "implementation":{}
   }]
}
```

There are two types of documents defined in the standard:

* **Releases** are immutable and represent updates on the contracting process. For example, they can be used to notify users of new tenders, awards, contracts and other updates. As such, a single contracting process can have many releases.

* **Records** are snapshots of the current state of a contracting process. A record should be updated every time a new release associated to its contracting process is published; hence, there should only be a single record per contracting process.

### Fields

Each section may contain several **fields** specified in the standard, which are used to represent data. These objects can appear several times in different sections of the same document; for example, items can occur in tender (to indicate the items that a buyer wishes to buy), in an award object (to indicate the items that an award has been made for) and in a contract object (to indicate the items listed in the contract). Some example fields, accompanied by corresponding JSON snippets, are presented below

#### Parties (Organizations)

```{json, eval=FALSE}
{
    "address": {
        "countryName": "United Kingdom",
        "locality": "London",
        "postalCode": "N11 1NP",
        "region": "London",
        "streetAddress": "4, North London Business Park, Oakleigh Rd S"
    },
    "contactPoint": {
        "email": "procurement-team@example.com",
        "faxNumber": "01234 345 345",
        "name": "Procurement Team",
        "telephone": "01234 345 346",
        "url": "http://example.com/contact/"
    },
    "id": "GB-LAC-E09000003",
    "identifier": {
        "id": "E09000003",
        "legalName": "London Borough of Barnet",
        "scheme": "GB-LAC",
        "uri": "http://www.barnet.gov.uk/"
    },
    "name": "London Borough of Barnet",
    "roles": [ ... ]
}
```

#### Amounts

```{json, eval=FALSE}
{
    "amount": 11000000,
    "currency": "GBP"
}
```

#### Items

```{json, eval=FALSE}
{
    "additionalClassifications": [
       {
            "description": "Cycle path construction work",
            "id": "45233162-2",
            "scheme": "CPV",
            "uri": "http://cpv.data.ac.uk/code-45233162.html"
        }
    ],
    "classification": {
        "description": "Construction work for highways",
        "id": "45233130",
        "scheme": "CPV",
        "uri": "http://cpv.data.ac.uk/code-45233130"
    },
    "description": "string",
    "id": "0001",
    "quantity": 8,
    "unit": {
        "name": "Miles",
        "value": {
            "amount": 137000,
            "currency": "GBP"
        }
    }
}
```

#### Time periods

```{json, eval=FALSE}
{
    "endDate": "2011-08-01T23:59:00Z",
    "startDate": "2010-07-01T00:00:00Z"
}
```

#### Documents

```{json, eval=FALSE}
{
    "datePublished": "2010-05-10T10:30:00Z",
    "description": "Award of contract to build new cycle lanes to AnyCorp Ltd.",
    "documentType": "notice",
    "format": "text/html",
    "id": "0007",
    "language": "en",
    "title": "Award notice",
    "url": "http://example.com/tender-notices/ocds-213czf-000-00001-04.html"
}
```

#### Milestones

```{json, eval=FALSE}
{
    "description": "A consultation period is open for citizen input.",
    "dueDate": "2015-04-15T17:00:00Z",
    "id": "0001",
    "title": "Consultation Period"
}
```


### Extensions and codelists

In addition to regular fields, the OCDS schema defines some fields that can only be used in certain sections, e.g. *titles* and *descriptions* of tenders, awards and contracts. In some cases, publishers may require fields that are not provided by the core schema; an **extension** allows defining new fields that can be used in these cases. A list of available extensions is available [here](http://standard.open-contracting.org/latest/en/extensions); if no existing extension addresses a publisher's needs, the publisher is encouraged to collaborate on the creation of a new community extension.

Another concept worth mentioning is that of codelists. Codelists are sets of case sensitive  strings with associated labels, available in each language OCDS has been translated into. Publishers should use codelist values whenever possible to map their existing classification systems; if needed, detail fields can be used to provide more detailed classification information. There are two types of codelists:

* **Closed codelists** are fixed sets of values. If a field is associated with a closed codelist, it should only accept an option from the published list.
* **Open codelists** are sets of recommended values. If a field is associated with an open codelist, it accepts options from the list but also other values.


OCDS is maintained using [JSON schema](http://json-schema.org). In this section we have introduced and described the main sections and common objects used in the schema, providing JSON snippets as examples of these basic building blocks. If you are interested in the full JSON schema reference, please refer to the [official documentation](http://standard.open-contracting.org/latest/en/schema/).
