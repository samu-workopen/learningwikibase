---
title: Visualize Data
description: Learn how to visualize your data and browse through examples.
class: visualisation
toc:
order: 6
image: /assets/images/articles/iconfinder_Makeup_379450.svg
---

There are different ways to visualize Wikibase data. The most common are:
- ...
- 

# Visualize wikidata data with Neo4j

[Neo4j](https://neo4j.com/) is a powerful tool which can be used to visualize graph data. Its [neosemantics plugin](https://neo4j.com/labs/neosemantics/) is helpfull for our need because it can map RDF data to Neo4j model. So it's possible to connect the wikibase SparQL endpoint to a Neo4j instance thanks to this plugin.

This article is very helpfull because it shows how to configure neosemantics to import a knowledge graph of monuments located in Spain into your local neo4j instance:
https://towardsdatascience.com/traveling-tourist-part-1-import-wikidata-to-neo4j-with-neosemantics-library-f80235f40dc5

Here is an example of the final result:
![image](https://user-images.githubusercontent.com/328244/116110957-74df9300-a6b6-11eb-8e46-29c22f16c630.png)

