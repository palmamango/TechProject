---
layout: page
title: Conclusions
permalink: /conclusions/
---

Our project has successfully enhanced the **ArCo Knowledge Graph** by enriching it with additional RDF triples related to Italy’s rich musical heritage. Through the use of SPARQL queries and advanced LLMs (such as LLaMa 3.1 and ChatGPT), we have addressed key gaps, particularly in the representation of musical instruments and their cultural significance. By connecting these elements with the historical event of *Tarantella*, notable liuthers and their construction techniques, we have contributed to a more interconnected and comprehensive digital representation of Italy’s cultural legacy. This work highlights the potential of combining **traditional querying techniques** with **modern AI-driven models**. Our use of innovative prompting strategies, such as *zero-shot, few-shot, chain-of-thought, and generated knowledge prompting*, has demonstrated the power of LLMs in both data extraction and generation, providing new insights for future academic and technological endeavors in the cultural heritage domain. 

### Main difficulties
During the project, we encountered several challenges, particularly in the **querying process**. One key issue was ensuring that the queries captured relevant and accurate data. However, in our initial results, such as in **query 1** and **2**, we noticed that it was impossible to find the entity of *Mandolino* in the `arco:MusicHeritage` class. This forced us to move to the `arco:MovableCulturalProperty` class as shown in **query 4**. This challenge highlighted the need for precision when dealing with complex and expansive datasets, where broader queries might miss crucial details or return irrelevant results. In fact, this helped us to create new RDF triples since *Mandolino* has revealed to be an entity with a lot of gaps to work on.

Furthermore, working with **LLMs** brought its own set of difficulties, as interpreting cultural data required careful adjustments to our prompting strategies. Ensuring consistency in the generated and extracted information also posed challenges, especially when integrating diverse historical and contemporary data points. 

The most exhaustive example is shown in **step 4**. We wanted to expand the familial connection in the page of Gaetano Vinaccia by using a prompt techinique with LLaMa 3.1. We realized that the information given was wrong by consulting the Wikipedia page for Vinaccia family. We, then, asked the same prompt to GPT-4, which provided us the correct information we needed to continue with our project. Therefore, we included Antonio and Pasquale entities, and linked them to Gaetano Vinaccia with a RDF triple, by using the predicate `a-cd:hasRelatedAgent`.
