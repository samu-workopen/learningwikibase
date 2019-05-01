---
title: FAQ
description: Share and add your learnings in the FAQ.
class: install-wikibase
toc:
  install-wikibase: "Install Wikibase"
  modeling-data: "Modeling Data"
  import-data: "Import Data"
  query-and-export-Data: "Query and Export Data"
  use-cases: "Use Cases"
order: 7
image: /assets/images/articles/iconfinder_Canoe_379522.svg
---

The questions and answers on this FAQ are Questions & Answers of Wikibase Workshops or the Telegram Channel of the Wikibase User Groups. When you have a questions or an answern, please contribute in the repo.

# Install Wikibase

### Where I can find an overview of all Wikibase Instances? 
There is the Wikibase registry: http://wikibase-registry.wmflabs.org/wiki/Main_Page
Its a wiki which hosts a registry of Wikibase instances. It’s volunteer based and in case that you have a Wikibase install to add please register and add your instance.

### What is Peacockbase? 
It’s a catalog for federated SPARQL Queries

### What is the recommended way of copying and moving LocalSettings.php (and what else may be left behind?) when updating the Wikibase Docker? 

So, local settings should already be outside of your container and loaded via a volume / mount, or you should be adding a layer to the base docker image with your customizations
If you haven't already got it in that state then you should be able to go in, grab the file and then mount it in a fresh container Update.php should work…

### What would be the recommended way of adding MediaWiki extensions in the Docker compose? Adding them in the Docker image or the deployed container?

So, the best way is to build up on top of the image, so make a custom image essentially with the specific extensions you also want laoded. So, the best way is to build up on top of the image, so make a custom image essentially with the specific extensions you also want laoded. The hacky way shoehorning them in can be read about at: https://addshore.com/2018/06/customizing-wikibase-config-in-the-docker-compose-example/

### When we run docker-compose up, does it create new containers or could it be that it restarts the containers which we have gotten into bad state (out-of-memory during update or something)?
docker-compose up is not guaranteed to create a new container. Maybe I should write a blog post about upgrading from the mediawiki side: https://addshore.com/2019/01/wikibase-docker-mediawiki-wikibase-update/

### How to upgrade a wikibase-docker?
https://docs.google.com/document/d/1vGhOvy1MXPPMOhBxrYWTRROP66X0HQmJZMvRV4bHed0/edit?usp=sharing
Please also check out: https://github.com/wmde/wikibase-docker/blob/master/README-compose.md and update the information there.

### I was concerned that building on top of the wikibase-bundle image wouldn't work because the commands would need to be run in the fetcher and composer instead of the final image, but perhaps that's not an issue after all?
So, you would end up having your own fetcher layer, and composer layer probably, and only in the final layer use FROM the wikibase-bundle. But that would result in less work than the rebasing approach with the same files.

### Where is the composer defined by the way?
HTTPS://hub.docker.com/_/composer and the wikibase ones are actually provided from Https://hub.docker.com/r/wikibase/wikibase if you don't build those layers yourself

### The old example yaml file referred to mariadb:latest which I suspect caused us to get a different version at some point. Wikibase image was built locally with "docker-compose build wikibase"?
Yup, the mariadb:latest will have caused your issue if you setup an entirely new set of containers, well only if you pulled the newer mariadb image in the process, which it sounds like you will have. Building the wikibase images yourself is technically fine, but if you want to run the exact same base as everyone else then you should probably build a layer on top with the modifications, and pull the wikibase image from docker hub rather than building

### How to use a custom Dockerfile?
https://goo.gl/A9yd2c

### Where do I find documentation about the new updater? 
This is important to e.g. provide a sync between blazegraph and wikibase.
Find documentation here: sudo docker ps and sudo docker logs.

### And the problem you have is which URLs are broken?
I think you want to configure the schema and host to what you want as it shows here: https://github.com/wmde/wikibase-docker/blob/master/wdqs/README.md

# Modeling Data

### Regarding federation in particular, how is this done for now? (By other Wikibase instances wanting to piggy back on Wikidata s ontology)
https://github.com/stuppie/wikibase-tools
...This is one solution where subsets of Wikidata properties are ingested and mappings are made
Unfortunatly the pid numbers from Wikidata can't be used yet. Individual wikibases will have to maintain their own id numbers and create mappings to Wikidata

### Is it possible to have different namespaces with items? Like one for local items and one for Wikidata items?

### Why does Wikidata have an item for each property? Is there a main canonical use case, given that Property is a DataType?

### Is there a canonical way to refer from an item to a template? If so, it would be very useful to get from ShEx to template and create a ShapeMap?

### Is there a way to get proper Wikidata autocomplete in the query service from the WDQS interface of a different Wikibase instance?

### Is there a canonical way to refer from an item to a template? If so, it would be very useful to get from ShEx to template and create a ShapeMap?

### How does the different prefixes work in wikibase?
This is a great diagram (and document) that explains how the different prefixes work, wish I saw it earlier: https://www.mediawiki.org/wiki/Wikibase/Indexing/RDF_Dump_Format#Data_model

### What’s the licensing scheme for contributions?
It currently doesn't have one :)
I would CC0 the data namespaces
Follow the Wikidata licensing schema: CC0 for all data + CC BY SA for the rest of the website

# Import Data

### What do you use for large scale imports in Wikidata? 
You can use WikidataIntegrator (https://github.com/SuLab/WikidataIntegrator)
A Wikidata Python module integrating the MediaWiki API and the Wikidata SPARQL endpoint.

### Is it possible to recreate a Wikibase from the RDF dump? 

### Is the authentification between the quickstatement and wikibase work on the docker image per default ?
...As far as I know it is not yet ready to work out of the box. There are detailed instructions in the README on how to set it up tohugh. Although... this is now merged so maybe it's further along than I thought https://github.com/wmde/wikibase-docker/pull/70/
The ticket for making it work out of the box is here https://phabricator.wikimedia.org/T205606

# Query and Export Data

### Is there any documentation on walking off the query interface only to logged in users when the Wikibase instance itself is set to private?
Nope not really, it would be fairly hard to do, you would have to lock the query API and UI behind some sort of http Auth thing on the Webserver probably

### Could wikibase/mediawiki act as a reverse proxy in front of the query service?
Yes

### Can we use services over query federation, e.g. send a sparql query to our own wikibase, which would connect to wikidata to perform "SERVICE wikibase:around" over there?
Yes you can. However, like wikidata, wikibase uses preselected endpoints you can federate against. This whitelist can be adapted, however I am currently using an empty blazegraph local endpoint to cater this use case.
I think you should be able to use the local blazegraph that comes packed with wikibase for that as well, but I actually don't know if that whitelist also applies on the blazegraph instance

### What is Sparql2?
SPARQL2 is a template in Wikidata that allows to store example SPARQL queries on Wikidata pages (e.g. the example page https://www.wikidata.org/wiki/Wikidata:SPARQL_query_service/queries/examples#Katten)

### I would so much like to have similar example pages on my on Wikibases
Currently adding an example SPARQL query to Wikidata is as simple as typing writing that query wihtin the SPARQL2 brackets
https://www.wikidata.org/wiki/Template:SPARQL2 after which you have a button to run the queries ???
The new daty wikidata editor release is out (editing still disabled though): https://www.wikidata.org/wiki/Wikidata:Project_chat#Daty_Wikidata_Editor_beta_release

### We have not succeeded using the Wikidata label service through a federated query. Is this a feature or a bug?
The label service seems quite magical so I suppose blazegraph is doing something special in the internals of the query processing and it might not be possible to federate with a remote sparql endpoint
The Wikidata label service is not standard SPARQL 1.1 so I don't expect it to work over federated queries. It is a specific hack for WDQS. However, if you change the query according to the following you can get the labels from the WDQS:

```
SELECT ?s ?sLabel WHERE {
  ?s wdt:Px ?o .
.........
.........
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
}


to 

SELECT ?s ?sLabel WHERE {
  ?s wdt:Px ?o ;
       rdfs:label ?sLabel .
  FILTER (lang(?s)="en")
.........
.........
}

````
Indeed lovely, but also expensive. Whenever I face a query that times out, the label service is the first to go to get better performance. workaround: 

```
  OPTIONAL { ?s rdfs:label ?lf . FILTER(LANG(?lf)=="fi") }

  OPTIONAL { ?s rdfs:label ?le . FILTER(LANG(?le)=="en") } 
  OPTIONAL { ?s rdfs:label ?ld . FILTER(LANG(?ld)=="") } 
  OPTIONAL { ?s rdfs:label ?la } 
  BIND(COALESCE(?lf,?le,?ld,?la) AS ?sLabel)

`````

### How can I configure the Query Service? 
https://www.mediawiki.org/wiki/Wikidata_Query_Service/User_Manual

# Maintain Wikibase

### How to build an interactive UI for viewing bulk load results to make corrections easier? 
You can use Shape Expressions. You define a schema and use the above mehtioned Simple Online Validator (https://rawgit.com/shexSpec/shex.js/wikidata/packages/shex-webapp/doc/shex-simple.html ) or http://rdfshape.weso.es/ and to see errors http://rdfshape.weso.es/

### Can I do a federation between 2 repros for e.g. interwiki prefixes
https://github.com/wikimedia/mediawiki-extensions-Wikibase/blob/master/docs/federation.wiki

### What's the recommendation for spam preparedness for my Wikibase Instance?
So, the main thing is having a catcha. The next thing is locking down the namespaces that you don't really use, most spam bots post to their user pages, and spambots mostly also only post to wikitext pages, not wikibase entities. That is ConfirmEdit for the captchas, and Nuke for cleaning up spam.
You might also find Abuse filter usefull in some situations but not really.

### How get sure that you maintain Wikibase right?
I always do full reloads and not incremental because we load in large bulk quantities and I wasn’t sure if incremental was made for that use case. I have code to load from spreadsheets, it’s on GitHub and uses the Java wikidata toolkit.

### How are you including the custom extensions right now? 
Official Way: 
So the best way to do that would be to create your own Docker file and at the top have FROM wikibase/wikibase-bundle:version-tag, then you don't need to deal with rebasing etc. I should try to convert the wikibase-registry to use a layer on top of the base image in the form of a extended DockerFile as an example and write something about it
Fork:
We forked wikibase-docker and added the ConfirmAccount plugin to wikibase/1.32/bundle/Dockerfile

### How to update in a most efficient way?
And for updates, really you want to update each thing separately, such as wikibase, mysql, wdws etc, not the whole lot at once. 

### Could mysql_update and update.php be run automatically when needed?
The need to run update.php is pretty hard to detect, and aromatically doing it could do unexpected things and unexpected times etc. As for mysql updating, the docs of the mariadb / mysql docker image on docker hub should be read about upgrading those, they are not custom at all.

### Has anyone experience with installing gadgets (e.g. Something like EasyQuery. It’s the official term, I thought. Certainly appears as one of the tabs in my Wikidata user profile.) on Wikibase? Any doc for that?
There is a gadgets extension, but then most of the gadgets you will need to copy and paste into your own wiki (at least now).  So you install the gadgets extension as a sysadmin, then manually copy the relevant js scripts as a wiki admin in the MediaWiki namespace. There are some tickets on phabricator about moving some gadgets into wikibase or an extension that can be included with it: https://phabricator.wikimedia.org/T205017 

### We only pulled the changes in wikibase-docker and rebuilt wikibase as that was the way we got ConfirmAccount in (without the extra layer, which would have been a better way). How is updating everything at once avoided? I thought all version upgrades were due to updates in the wikibase-docker release.
So really you don't want to be using the got repo at all, you just want to create your own docker-compose.yml file and work from that. The one in the repo is only meant as an example, and of you just pull in changes without expecting them I expect things will break most of the time.

### Can someone help me out? I am trying to use the Special:MyPage/common.JS framework on my own Wikibase instance but no luck. The MediaWiki:common.JS thing works on it though, and I presume I understood how it works as a user since I can get it to work on Wikidata. Is there a flag that needs activation?
You need to set $wgAllowUserJs to true :)
https://www.mediawiki.org/wiki/Manual:$wgAllowUserJs

### Can someone help me out? I am trying to use the Special:MyPage/common.JS framework on my own Wikibase instance but no luck. The MediaWiki:common.JS thing works on it though, and I presume I understood how it works as a user since I can get it to work on Wikidata. Is there a flag that needs activation?
You need to set $wgAllowUserJs to true :)
https://www.mediawiki.org/wiki/Manual:$wgAllowUserJs


# Use Cases

### Where can I find more information about the Wikibase Usergroup? 
https://meta.wikimedia.org/wiki/Wikibase_Community_User_Group/Reports/2018
















