# IYP Gallery

Below are examples queries that you can run in Neo4j browser. 

Simple queries:
1. [Names for AS2497](#names-for-as2497)
2. [All nodes related to 8.8.8.0/24](#all-nodes-related-to-8.8.8.0/24)
3. [Country code of AS2497 in delegated files](#country-code-of-as2497-in-delegated-files)
4. [Countries of IXPs where AS2497 is present](#countries-of-ixps-where-AS2497-is-present)
5. [Countries of IXPs where AS2497 is present](#countries-of-ixps-where-as2497-is-present)


## Simple queries

### Names for AS2497
Find 'NAME' nodes directly connected to the node corresponding to AS2497.
```
MATCH (a:AS {asn:2497})--(n:NAME) RETURN a,n
```
![Names for AS2497](/documentation/assets/gallery/as2497names.svg)

### All nodes related to 8.8.8.0/24
Find nodes of any type that are connected to the node corresponding to prefix 
8.8.8.0/24.
```
MATCH (gdns:PREFIX {prefix:'8.8.8.0/24'})--(neighbor) 
RETURN gdns, neighbor
```

![All nodes related to 8.8.8.0/24](/documentation/assets/gallery/prefixAllRelated.svg)

### Country code of AS2497 in delegated files
Here we search for a country node directly connected to AS2497's node and that
comes from NRO (the organization providing delegated files to IYP).
```
MATCH (iij:AS {asn:2497})-[{reference_org:'NRO'}]-(cc:COUNTRY) 
RETURN iij, cc
```

![Country code of AS2497 in delegated files](/documentation/assets/gallery/as2497country.svg)


### Countries of IXPs where AS2497 is present
Starting from the node corresponding to AS2497, find IXPs where AS2497 is member
of, and then the country corresponding to each IXP.
```
MATCH (iij:AS {asn:2497})-[:MEMBER_OF]-(ix:IXP)--(cc:COUNTRY) 
RETURN iij, ix, cc
```

![Countries of IXPs where AS2497 is present](/documentation/assets/gallery/as2497ixpCountry.svg)

### Top domain names hosted by AS2497
Select domain names in top 50k rankings that resolves to an IP originated by
AS2497.

```
MATCH (:RANKING)-[r:RANK]-(dn:DOMAIN_NAME)--(ip:IP)--(pfx:PREFIX)-[:ORIGINATE]-(iij:AS {asn:2497})
WHERE r.rank < 50000
RETURN dn, ip, pfx, iij
```

![Top domain names hosted by AS2497](/documentation/assets/gallery/as2497domainNames.svg)