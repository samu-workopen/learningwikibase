---
title: Import Data
description: Learn how to import data to your Wikibase. 
class: data-import
toc:
order: 3
image: /assets/images/articles/data-import.svg
---
The main entrypoint to adding data to Wikibase is through its user interface. Here adding data is straightforward. Data is added statement by statement, reference by reference and qualifier by qualifier. Although straightforward this approach does not scale. For bulk imports of data into a Wikibase one has to rely on the api available at <wikibase main URL>/w/api.php

Different tools and libraries have been developed providing a wrapper around the wikibase api of Wikidata. Some of those can also be used in any deployed Wikibase. In this chapter 2 will be addressed, quickstatements and the Wikidata Integrator

# QuickStatements
QuickStatements is a tool developed for Wikidata to create items and add statements. It uses a syntax of TAB separated constructs. QuickStatements is also packed in the docker-compose script, which enables using quickstatements on the Wikibase it is packed with on that docker run stack. Once deployed go to <wikibase main URL>:9191 to access the quickstatement module of that Wikibase. For details and instructions on how to use quickstatement on the installed wikibase click on the HELP button on the top of the main page.

# Wikidata integrator (WDI)
The Wikidata integrator is a python library developed to create and maintain data on Wikidata. The Wikidata creator depends on both the mediawiki main api of Wikibase and the SPARQL server available through the WBQS built on top of the main api. 

## install WDI
The Wikidata integrator depends on python version 3.6 and higher. 
To install the WDI library type the following command on the command line

```bash
pip3 install wikidataintegrator
```

Once the library is installed a wikibase bot can be developed to upload and maintain content on an installed wikibase. 

### Code snippets
First import the wikidata integrator
```python
from wikidataintegrator import wdi_core, wdi_login
```
Then provide login credentials for the wikibase
```python
logincreds = wdi_login.WDLogin(user="<wbusername>", pwd="<wbpassword>", mediawiki_api_url="<wikibase_url>:8181/w/api.php")
```
In the above snippet the wbusername/wbpassword is hardcoded in the code, which can be considered bad practice. Code can end up in public repositories. 
That is why the following code is a safer sollution. Here sensitive data is stored in local environment variable, which need to be set outside the python script.

```python
from wikidataintegrator import wdi_core, wdi_login
import os 

logincreds = wdi_login.WDLogin(user=os.environ["wd_user"], pwd=os.environ["pwd"])
```

The next step is to prepare the data that needs to be loaded into the fresh wikibase. Here I am using an excel table as an example, but in principle any data format goes, as long as it can be parsed by a python script. The wikidata integrator is developed in then Gene Wiki project, where the aim is to contribute public research data from the life sciences to Wikidata. In this project data stored in XML, RDF, csv, and a variety of live APIs is processed into wikidata. 

As said for the sake of this tutorial an excel table is used. To parse the excel table the pandas library is used. 

```python
# load the necessary libraries
from wikidataintegrator import wdi_core, wdi_login
import pandas as pd

# login to wikibase
logincreds = wdi_login.WDLogin(user=os.environ["wd_user"], pwd=os.environ["pwd"])
# load excel table to load into Wikibase
mydata = pd.read_excel("mydata.xlsx")
```

When the data is loaded it can be added to the wikibase. 
If the properties do not exist yet, they need to be created. These properties can be also added using the WDI, however for this tutorial we will manually create the properties using the web interface of the the wikibase. To do this go to <wikibaseURL>:8181/wiki/Special:NewProperty.

For this tutorial lets assume there are the following properties set: 
P1: column1 type: String
P2: column2 type: Item
P3: column3 type: URL
WDI supports more data types, for details see the WDI documentation

Asume that these properties reflect the columns in "mydata.xlsx". Adding the content in the excel table is now done with the following python script

```python
...
for index, row in mydata.iterrows():
	item_statements = [] # all statements for one item
	data.append(wdi_core.WDString("column1_value", prop_nr="P1")) 
	data.append(wdi_core.WDItem("Q1234", prop_nr="P2"))
	data.append(wdi_core.WDURL("<http://someURL>", prop_nr="P3")) 
``` 
Once the different data elements are converted to statements as done above the page can be instantiated and its labels and descriptions can be set.

```python
...
wbPage = wdi_core.WDItemEngine(data=data, mediawiki_api_url="<wikibaseURL>:8181/w/api.php")
    wbPage.set_label("label", lang="en")
    wbPage.set_label("naam", lang="nl")
    wbPage.set_label("nom", lang="fr")
    wbPage.set_description("description", lang="en")
    wbPage.set_description("beschrijving", lang="nl")
    wbPage.set_description("description", lang="fr")
```

Everything is now set to submit to your wikibase. Before you do you might want to check the content of the data before it is submitted. 

```python
...
pprint.pprint(wbPage.get_wd_json_representation())
```
The data is submitted with the following line
```python
...
wbPage.write(logincreds)
```

## complete bot
```python
# load the necessary libraries
from wikidataintegrator import wdi_core, wdi_login
import pandas as pd
import pprint

# login to wikibase
logincreds = wdi_login.WDLogin(user=os.environ["wd_user"], pwd=os.environ["pwd"])
# load excel table to load into Wikibase
mydata = pd.read_excel("mydata.xlsx")
for index, row in mydata.iterrows():
	## Prepare the statements to be added
	item_statements = [] # all statements for one item
	data.append(wdi_core.WDString("column1_value", prop_nr="P1")) 
	data.append(wdi_core.WDItem("Q1234", prop_nr="P2"))
	data.append(wdi_core.WDURL("<http://someURL>", prop_nr="P3"))

	## instantiate the Wikibase page, add statements, labels and descriptions
	wbPage = wdi_core.WDItemEngine(data=data, mediawiki_api_url="<wikibaseURL>:8181/w/api.php")
    wbPage.set_label("label", lang="en")
    wbPage.set_label("naam", lang="nl")
    wbPage.set_label("nom", lang="fr")
    wbPage.set_description("description", lang="en")
    wbPage.set_description("beschrijving", lang="nl")
    wbPage.set_description("description", lang="fr")

    ## sanity check
    pprint.pprint(wbPage.get_wd_json_representation())

    ## import
    wbPage.write(logincreds)
```

This is a first bot. WDI has more features, such as adding qualifiers, references, using ShEx, using SPARQL. In this bot only data is added. However, WDI is mainly used to keep Wikidata in sync with its primary source. This applies to Wikibase as well. More details on how to used these feature will follow, but for now please rely on the documentation provided on WDI github page. 





