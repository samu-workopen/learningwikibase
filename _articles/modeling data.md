---
title: Data & Modeling Data
description: Learn how to model data.
class: install-wikibase
toc:
  Linked-Data: "Linked Data"
  RDF-Shape: "RDF Shape"
  Linked-Data-Modeling: "Linked Data Modeling"
  Shape-Expressions: "Shape-Expressions"
  Decription-and-validation-of-Data: "Description and validation of Data"
  Data-model-visualisation: "Data model visualisation"
  
order: 2
image: /assets/images/articles/iconfinder_Dna_379486.svg
---
Content for this website will follow.

# Linked Data
Wikibase is a software stack that allows storing linked data. Linked data is data that follows two principles. 
1. Data is available as [sematic triples](https://en.wikipedia.org/wiki/Semantic_triple).
2. Data points use [Internationlized Resource Identifiers (IRI)](https://en.wikipedia.org/wiki/Internationalized_Resource_Identifier)

## Semantic triples
Core to link data is the concept of semantic triples. In relational databases, data is stored in tables. Spreadsheet also use a tabular structure to capture data. A semantic triple, however can be seen as a statement. 

The following record in tabular data
|book title|author|
|--|--|
| The origin of species | Charles Darwin  |
can also be described as the following semantic triple in linked data
```mermaid
graph LR
  book[The origin of species] -- has author --> author[Charles Darwin]
```
The benefit of having the data as triples is that combining various datasets becomes straight forward. 
If we want to combine our book table with the following data on the author.

|person| data of birth | birthplace
|--|--|--|
|Charles Darwin  | 12 February 1809 | [The Mount](https://en.wikipedia.org/wiki/The_Mount,_Shrewsbury "The Mount, Shrewsbury"), [Shrewsbury](https://en.wikipedia.org/wiki/Shrewsbury "Shrewsbury"), [Shropshire](https://en.wikipedia.org/wiki/Shropshire "Shropshire"), England |



# RDF Shape

# Linked Data Modelling

# Shape Expression (ShEx)

# Description and validation of Data

# Data model visualisation

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwNzA0NDc2OSwxMjE1NzM5MDEsLTExMz
Y0NDYzOF19
-->