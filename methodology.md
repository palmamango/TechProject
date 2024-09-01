---
layout: page
title: Methodology
permalink: /methodology/
---

In our project, we aimed to enrich the [ArCo Knowledge Graph](http://wit.istc.cnr.it/arco/?lang=en) by identifying and connecting underrepresented or missing cultural elements related to Italian musical heritage, particularly focusing on traditional instruments and their cultural contexts. Below, we outline the step-by-step process that we followed, from querying existing data to creating new RDF triples.

## Step 1: Querying the ArCo Knowledge Graph

### 1.1 Listing Elements of the `arco:MusicHeritage` Class

We began by exploring the `arco:MusicHeritage` class to list its elements. The goal was to identify all entities classified under this class, with a particular focus on musical instruments. We limited the results to the first 50 entries, ordered alphabetically by their labels.

```sparql
PREFIX arco: <https://w3id.org/arco/ontology/arco/>
SELECT DISTINCT ?instrument ?label
WHERE {
  ?instrument rdf:type arco:MusicHeritage ;
              rdfs:label ?label .
}
ORDER BY (?label)
LIMIT 50
```

### 1.2 Searching for Specific Musical Instruments

Next, we focused on finding specific musical instruments by searching for entities containing the terms *"mandolino"*, *"tamburello"* or *"zampogna"* in their labels. This query used the `UNION` clause to combine results for these three instruments and was also limited to 50 results.


```sparql
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?instrument ?label
WHERE {
  {
    ?instrument rdf:type arco:MusicHeritage ;
                rdfs:label ?label .
    FILTER(REGEX(?label, "mandolino", "i"))
  }
  UNION
  {
    ?instrument rdf:type arco:MusicHeritage ;
                rdfs:label ?label .
    FILTER(REGEX(?label, "tamburello", "i"))
  }
  UNION
  {
    ?instrument rdf:type arco:MusicHeritage ;
                rdfs:label ?label .
    FILTER(REGEX(?label, "zampogna", "i"))
  }
}
ORDER BY (?label)
LIMIT 50
```