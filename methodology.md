---
layout: page
title: Methodology
permalink: /methodology/
---

In our project, we aimed to enrich the [ArCo Knowledge Graph](http://wit.istc.cnr.it/arco/?lang=en) by identifying and connecting underrepresented or missing cultural elements related to Italian musical heritage, particularly focusing on traditional instruments and their cultural contexts. Below, we outline the step-by-step process that we followed, from querying existing data to creating new RDF triples.

## Step 1: Querying the ArCo Knowledge Graph

### 1.1 Listing Elements of the arco:MusicHeritage Class

We began by exploring the `arco:MusicHeritage` class to list its elements. The goal was to identify all entities classified under this class, with a particular focus on musical instruments. We limited the results to the first 50 entries, ordered alphabetically by their labels.

#### Query 1

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

The results of this query are visitable at the following URL: [MusicHeritage Query Results](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0ASELECT+DISTINCT+%3Finstrument+%3Flabel%0D%0AWHERE+%7B%0D%0A++%3Finstrument+rdf%3Atype+arco%3AMusicHeritage+%3B%0D%0A++++++++++%09rdfs%3Alabel+%3Flabel+.%0D%0A++++++++%09+%0D%0A%7D%0D%0AORDER+BY+%28%3Flabel%29%0D%0ALIMIT+50%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).

### 1.2 Searching for Specific Musical Instruments

Next, we focused on finding specific musical instruments by searching for entities containing the terms *"mandolino"*, *"tamburello"* or *"zampogna"* in their labels. This query used the `UNION` clause to combine results for these three instruments and was also limited to 50 results.

#### Query 2

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

The results of this query are visitable at the following URL: [Specific Musical Instruments Query Results](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Finstrument+%3Flabel%0D%0AWHERE+%7B%0D%0A++%7B%0D%0A%09%3Finstrument+rdf%3Atype+arco%3AMusicHeritage+%3B%0D%0A++++++++++++%09rdfs%3Alabel+%3Flabel+.%0D%0A%09FILTER%28REGEX%28%3Flabel%2C+%22mandolino%22%2C+%22i%22%29%29%0D%0A++%7D%0D%0A++UNION%0D%0A++%7B%0D%0A%09%3Finstrument+rdf%3Atype+arco%3AMusicHeritage+%3B%0D%0A++++++++++++%09rdfs%3Alabel+%3Flabel+.%0D%0A%09FILTER%28REGEX%28%3Flabel%2C+%22tamburello%22%2C+%22i%22%29%29%0D%0A++%7D%0D%0A++UNION%0D%0A++%7B%0D%0A%09%3Finstrument+rdf%3Atype+arco%3AMusicHeritage+%3B%0D%0A++++++++++++%09rdfs%3Alabel+%3Flabel+.%0D%0A%09FILTER%28REGEX%28%3Flabel%2C+%22zampogna%22%2C+%22i%22%29%29%0D%0A++%7D%0D%0A%7D%0D%0AORDER+BY+%28%3Flabel%29%0D%0ALIMIT+50%0D%0A%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).


### 1.3 Isolating "Mandolino" Results
Upon reviewing the results of [Query 2](#query-2), we noticed that there were no relevant entries for *'mandolino'*. To ensure accuracy, we decided to refine the query by isolating only the term *'mandolino'* and executing it again.


#### Query 3

```sparql
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?instrument ?label
WHERE {
  ?instrument rdf:type arco:MusicHeritage ;
              rdfs:label ?label .
  FILTER(REGEX(?label, "mandolino", "i"))
}
```

The results of this query are visitable at the following URL: [Mandolino Query Results](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Finstrument+%3Flabel%0D%0AWHERE+%7B%0D%0A%0D%0A%09%3Finstrument+rdf%3Atype+arco%3AMusicHeritage+%3B%0D%0A++++++++++++%09rdfs%3Alabel+%3Flabel+.%0D%0A%09FILTER%28REGEX%28%3Flabel%2C+%22mandolino%22%2C+%22i%22%29%29%0D%0A+%0D%0A+%7D%0D%0A&format=text%2Fhtml&timeout=0&signal_void=on).


### 1.4 Expanding the Search to arco:MovableCulturalProperty

Finding no results under the `arco:MusicHeritage` class, as shown in [Query 3](#query-3), we expanded our search to include the parent class, `arco:MovableCulturalProperty`. This query successfully identified several instances of *'mandolino'* under this broader category, although some of these instances did not specifically refer to a musical instrument.


#### Query 4

```sparql
PREFIX arco: <https://w3id.org/arco/ontology/arco/>

SELECT DISTINCT ?instrument ?label
WHERE {
  ?instrument rdf:type arco:MovableCulturalProperty ;
              rdfs:label ?label .
  FILTER(REGEX(?label, "mandolino", "i"))
}
```

The results of this query are visitable at the following URL: [MovableCulturalProperty Query Results](https://dati.cultura.gov.it/sparql?default-graph-uri=&query=PREFIX+arco%3A+%3Chttps%3A%2F%2Fw3id.org%2Farco%2Fontology%2Farco%2F%3E%0D%0A%0D%0ASELECT+DISTINCT+%3Finstrument+%3Flabel%0D%0AWHERE+%7B%0D%0A++%3Finstrument+rdf%3Atype+arco%3AMovableCulturalProperty+%3B%0D%0A++++++++++++++rdfs%3Alabel+%3Flabel+.%0D%0A++FILTER%28REGEX%28%3Flabel%2C+%22mandolino%22%2C+%22i%22%29%29%0D%0A%7D&format=text%2Fhtml&timeout=0&signal_void=on).


One such result, for example, was:

- [Mandolino: Chiocca Filippo](https://dati.beniculturali.it/lodview-arco/resource/HistoricOrArtisticProperty/1200066104.html)


![Image of Chiocca Filippo's Mandolin](./img//chiocca-mandolin-xs.jpg)




## Creating RDF Triples to Enrich the ArCo KG

### 2.1 Assigning the Correct Class

Upon identifying an entity representing the mandolin by Chiocca Filippo (as well as the other mandolins), we noted that it lacked a connection to the arco:MusicHeritage class. To address this, we created new RDF triples to establish this link:

#### Triple 1

```rdf
<https://w3id.org/arco/resource/HistoricOrArtisticProperty/1200066112>
    
    rdf:type

<https://w3id.org/arco/ontology/arco/MusicHeritage> 
```

#### Triple 2

```rdf
<https://w3id.org/arco/resource/HistoricOrArtisticProperty/1200066121>
    
    rdf:type
    
<https://w3id.org/arco/ontology/arco/MusicHeritage>
```
In the second triple, we performed the same operation as in the first one, but with a different mandolin: the [mandolin by Iovine Antonio (18th/19th century)](https://dati.beniculturali.it/lodview-arco/resource/HistoricOrArtisticProperty/1200066121.html).


### 2.2 Linking Instruments to Cultural Events

Before performing the SPARQL query, we utilized a Large Language Model (LLM) to explore potential historical events or cultural practices in Italy that could be linked to the mandolin. The prompt we used in the LLM was:

> ### Prompt:
> What historical events or cultural practices in Italy could be linked to the mandolin? Let’s think step by step.


The LLM, specifically GPT-4o, provided a comprehensive response, outlining various cultural and historical aspects related to the mandolin in Italy. 
We employed a **Chain of Thought prompting technique**, specifically using a [zero-shot approach combined with Chain of Thought reasoning](https://www.promptingguide.ai/techniques/cot#zero-shot-cot-prompting). This technique allowed the model to generate a step-by-step analysis, ensuring that the connections made were logical and detailed.

Here’s the response we received:

> ### LLM Response:
> 
> #### Origins and Development of the Mandolin
> 
> - **Lute Family:** The mandolin is a descendant of the lute family, which has roots in various ancient stringed instruments from the Mediterranean and Middle Eastern regions. The mandolin, as we recognize it today, started to take shape in Italy during the 17th and 18th centuries.
>
> - **Neapolitan Mandolin:** The most recognized form of the mandolin is the Neapolitan mandolin, developed in Naples in the 18th century. The city became a major center for the production of mandolins, influencing the design and sound of the instrument globally.
> 
> #### Cultural and Social Influence in Italy
> 
> - **Folk Music:** The mandolin became a key instrument in Italian folk music, particularly in southern Italy. It was used in traditional dances like the Tarantella, a lively folk dance that has roots in ancient healing rituals.
>
> - **Opera and Classical Music:** The mandolin also found its place in classical music. Composers like Vivaldi and Mozart wrote pieces for the mandolin, integrating it into the broader European classical music tradition. Vivaldi's mandolin concertos are particularly well-known.
>
> - **Serenades and Courtship:** In Italian culture, especially during the 18th and 19th centuries, the mandolin was often associated with serenades. A young man might use the mandolin to serenade his beloved, a practice that became a romantic symbol in Italy.
> 
> #### Historical Events and Mandolin Influence
> 
> - **Italian Unification (Risorgimento):** During the 19th century, the period of the Italian unification, the mandolin was a popular instrument among the populace. Folk songs and ballads played on the mandolin often carried themes of national identity and unity, contributing to the cultural fabric of the Risorgimento.
>
> - **Migration and Global Influence:** The late 19th and early 20th centuries saw large waves of Italian emigration, particularly to the Americas. Italian immigrants brought the mandolin with them, influencing the music of places like the United States, where the mandolin became a fixture in bluegrass music and other genres.
> 
> #### 20th Century and Popular Culture
> 
> - **Cinema and Music:** In the 20th century, the mandolin continued to symbolize Italian culture, appearing in films and music. Italian cinema often used the mandolin to evoke a sense of nostalgia or to underscore scenes of traditional Italian life.
>
> - **Modern Italian Folk Revival:** The mandolin saw a resurgence in the late 20th century as part of a broader revival of interest in folk music. This period saw renewed appreciation for traditional Italian music and instruments, with the mandolin often at the center.
> 
> #### Contemporary Italian Cultural Practices
> 
> - **Music Festivals:** Today, the mandolin is celebrated in numerous music festivals across Italy, particularly in regions like Campania and Sicily. These festivals often focus on traditional music, but also showcase modern compositions for the mandolin.
>
> - **Mandolin Orchestras:** In some Italian regions, mandolin orchestras are a significant cultural institution, preserving the art of mandolin playing and continuing its development into contemporary music genres.
> 
> **Summary:** The mandolin is deeply intertwined with Italian history and culture, from its origins in the lute family to its role in folk music, classical compositions, and national identity during the Risorgimento. It has been a symbol of Italian romance, a tool for political expression, and a cultural ambassador as Italians migrated worldwide. Today, the mandolin remains a vibrant part of Italy's musical landscape, celebrated in festivals and performances that honor its historical and cultural significance.

<u>At this <a href="./img/cot.png">link</a> you can find a screenshot of the prompt we used.</u>


---


Based on this response, we identified the Tarantella as a key cultural event associated with the mandolin. This insight led us to query the ArCo Knowledge Graph to locate the entity representing the Tarantella: