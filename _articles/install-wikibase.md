---
title: Install Wikibase
description: Learn how to install Wikibase using Git and Composer, using the Wikibase Docker image or Open Stack
class: install-wikibase
toc:
install-wikibase-natively-with-git-and-composer: "Install Wikibase natively with Git and Composer"
install-wikibase-with-docker-: "Install Wikibase with Docker"
open-stack-to-run-a-custom-wikibase-: "Install Wikibase with OpenStack"
order: 1
image: /asset/images/articles iconfinder_Coding-Html_379494.svg
---

You will find different ways on how to install Wikibase. The most common are:

- [Git and Composer](https://www.mediawiki.org/wiki/Wikibase/Installation#Vagrant) 
- [Wikibase Docker image](https://github.com/wmde/wikibase-docker/blob/master/README-compose.md)
- [Openstack](https://fuga.cloud/labs/using-openstack-to-run-custom-wikibase/)

When you install Wikibase and your Wikibase Installation is public please register you installation in the [Wikibase Registry](http://wikibase-registry.wmflabs.org/wiki/Main_Page)

## Install Wikibase natively with Git and Composer 

This video will show you a screencast on how to install Wikibase natively with Git and Composer:

Video 

Prerequisite to install a Wikibase instance:
- Web server
- Git as tools (version control system) 
- composer

Prerequisite to run MediaWiki is that you have a server Media Wiki Prerequisite (apachee local server you have to remotely try out a server). There are several distributions. Best known is [Apachefriends](https://www.apachefriends.org/index.html) . 

### Clone MediaWiki repository 

1. Set Up MediaWiki in your terminal.
2. You can clone the repository and click on the branch you want. The different versions are in the different branches: [Download from Git](https://www.mediawiki.org/wiki/Download_from_Git).
4. We tried version 1.30 to test the updates. Normally you should always use the latest version to make it stable and have the latest features. (If you took the branch away, you'd take the master, but it's currently under development). Information about current releases (newer versions of Wikibase) can be found [here](https://www.mediawiki.org/wiki/Release_notes).
5. Create a clon of the wikibase version you need with [the command](https://gerrit.wikimedia.org/r/mediawiki/core.git --branch REL1_30 mediawiki) in your command line.
6. When you create a MediaWiki Folder on your device, you can put this clone into your MediaWiki folder.  
7. As next step you can get all extensions you want to install. This is what you do with Composer. The extensions must match the MediaWiki version you downloaded. The extensions are all stored on packages. You can find them at [List of Extensions](https://gerrit.wikimedia.org/r/#/admin/projects/?filter=mediawiki%252Fextensions%252F). 
For example: when you install the vector skin by typing the following command into your command bar: 

```composer require mediawiki/vector-skin:dev-REL1_30```

The composer automatically loads components (packages) from which the packages you install indirectly depend, e.g. the vector skin needs more packages to work. These can be found in his MediaWikiOrdnung under the "Vendor" directory. In the Composer Jason File all dependencies are defined which are needed for the project. You want to achieve that you always have the latest version.  

### Add extensionsion using composer . 

The whole extensions can also be cloned without the composer, but with the composer it is easier.
2. now you have to install all the dependencies of Media Wiki yourself e.g. Debugging
3. Now you have your web server. You can now install MediaWiki. (You have two paths (the local web server directly on the computer and the web directory, which is external) → you access the directory you have cloned MediaWiki into via your browser.
Then you execute the MediaWiki installation routine → You are led through a menu. 
5. then you can access your wiki. Now you can see that the vector skin (extension) is already inside automatically.   

### Configure Wikibase . 

1. If you click on "Special Version" → you need to activate Wikibase Language Version (you need this to create labels in different languages) and Wikibase Repro (you need this to create items and properties on your Wikibase instance) → is also located in the Docker Image.
2. You need to activate the local settings. To do this you have to copy the LoW Extension command into the Local Settings PHP version. The file will be generated when you finish the MediaWiki installation, then copy it to your main directory. The command is: wfLoadExtension( 'UniversalLanguageSelector' ). 
The activation of Wikibase is done from the [Wikibase installation page](https://www.mediawiki.org/wiki/Wikibase/Installation) e.g. you can only activate the repro if only this is needed e.g. depending on the purpose (e.g. you can decide if the client is needed or not)
4. If you've updated your special version now, you're ready to go. If you use this command: php maintenance/update.php.
5. Executes all basic things will be created (e.g. the database tables you need to work in Wikibase)
6. The wikibase repro you have installed should work this way.   

## Install Wikibase with Docker .  

This Video will show you a Docker Image Installation:     

VIDEO IN PROGRESS . 

All informations about the Docker installation you can find [Readme-Compose](https://github.com/wmde/wikibase-docker/blob/master/README-compose.md).

## Installing a stand-alone Wikibase instance + SPARQL Query Service with Docker

This repository contains an EXAMPLE docker compose file that can be used with the images also contained in this repo. Following these instructions, you'll be able to install a stand-alone instance of Wikibase – the collaborative structured data engine behind [Wikidata](https://wikidata.org/) – as well as fully functional SPARQL endpoint and query service, complete with a data visualization frontend and query helper (similar to the [Wikidata Query Service](https://query.wikidata.org/)).

Individual documentation for each image used in this example can be found [here](https://github.com/wmde/wikibase-docker/blob/master/README.md).

**WARNING:** Currently this example requires 3GB+ of memory. If you are running Docker for Windows or Mac and have containers run within a VM (the default) make sure you set your VM memory allocation to 4GB. If you don't do this the query service may fail to run.

## Installing Docker and the Wikibase images

You'll need to install Docker, Docker Compose and clone images from this repository. For a single script to run that should work on a Wikimedia Toolforge (wmflabs) VM running Debian Jessie with 4GB memory, take a look at setup.sh in this repository.

#### 1) Install Docker (community edition) & Docker compose

 - https://docs.docker.com/engine/installation/
 - https://docs.docker.com/compose/install/

Note: Docker Compose is included in the Docker community edition, so most likely you won't need to install it separately.

#### 2) Copy the docker-compose.yml file

This file is only meant as an example, your modification might be needed.

```
wget https://raw.githubusercontent.com/wmde/wikibase-docker/master/docker-compose.yml
```

If you want to develop the Dockerfiles then you should clone this repo and instead use the docker-compose-build.yml file.

#### 3) Setting up and starting the containers

Note: In linux you must either be in the docker group or use SUDO!

**Pulling / updating the images**

```
docker-compose pull
```

**Starting the containers**

```
docker-compose up
```

**Quickstatements**
To set up quickstatements follow the instructions in the quickstatements [README](https://github.com/wmde/wikibase-docker/blob/master/quickstatements/README.md)

**Stopping the containers**

```
docker-compose stop
```

**Removing the containers**

```
docker-compose down
```

This will keep all data stored by MySQL, Mediawiki and the Query Service in Docker volumes.

**Removing both the containers and data**

**WARNING:** this will remove ALL of the data you had added to Wikibase and the Query Service.

```
docker-compose down --volumes
```

## Backing up data from Docker volumes

All data for Wikibase and the Query Service is stored in Docker volumes. You can create compressed copies of these volumes to use as backups or to hand off to other users.

Volume backups will only work if you use the same image when restoring / using the backup data.
If you are backing up your mysql data you may also want to just take an SQL dump.

You can see all Docker volumes created by using the following command:

```
docker volume ls | grep wikibase-docker
```

You can grab a zip of each volumes by doing the following:

```
docker run -v wikibase-docker_mediawiki-mysql-data:/volume -v /tmp/wikibase-data:/backup --rm loomchild/volume-backup backup mediawiki-mysql-data
```

You, or someone else, can then restore the volume by doing the following:

```
docker run -v wikibase-docker_mediawiki-mysql-data:/volume -v /tmp/wikibase-data:/backup --rm loomchild/volume-backup restore mediawiki-mysql-data
```

## Backing up data using mysqldump

If using volume-backup for the database does not work because of InnoDB tables
([check here](https://dev.mysql.com/doc/refman/8.0/en/backup-methods.html)),
you can achieve data backup using mysqldump.

### Backing up with mysqldump

```
docker exec wikibase-docker_mysql_1 mysqldump -u wikiuser -psqlpass my_wiki > backup.sql
```

### Restoring from mysqldump backup file

```
docker exec wikibase-docker_mysql_1 mysql -u wikiuser -psqlpass my_wiki < backup.sql

```

## Accessing your Wikibase instance and the Query Service UI

Access the following hosts:
 - [Wikibase @ http://localhost:8181](http://localhost:8181)
 - [Query Service UI @ http://localhost:8282](http://localhost:8282)
 - [Query Service Backend (Behind a proxy) @ http://localhost:8989/bigdata/](http://localhost:8989/bigdata/)
 - [Query Service Backend (Direct) @ http://localhost:8999/bigdata/](http://localhost:8999/bigdata/)
 - [Quickstatements @ http://localhost:9191](http://localhost:9191)

## Creating content and querying it

You can start creating items and properties to start populating your Wikibase instance via these special pages
  - [Create a new item](http://localhost:8181/wiki/Special:NewItem)
  - [Create a new property](http://localhost:8181/wiki/Special:NewProperty)

Once you've created your first items and statements, you'll be able to query them and visualize the results through the [Query Service UI](http://localhost:8282).

## Exporting data as a JSON or RDF dump

Get a JSON dump from wikibase:

```docker-compose exec wikibase php ./extensions/Wikibase/repo/maintenance/dumpJson.php```

Get an RDF dump from wikibase:

```docker-compose exec wikibase php ./extensions/Wikibase/repo/maintenance/dumpRdf.php```

## Troubleshooting

* [I am on linux and I don't want to run docker as root!](https://askubuntu.com/questions/477551/how-can-i-use-docker-without-sudo#477554)
* The query service is not running or seems to get killed by the OS?
  * The docker-compose setup requires more than 2GB of available RAM to start. While being developed the dev machine has 4GB of RAM.


## Open Stack to run a custom Wikibase . 

This Video will show you, how to do run a custom Wikibase with OpenStack.

VIDEO IN PROGRESS

To get all information please read [Using Open Stack to run custom Wikibase](https://fuga.cloud/labs/using-openstack-to-run-custom-wikibase/) . 

### Getting Started .    

1. Create an account, if you don’t have one already
2. Add billing details
3. Click the button to setup horizon
4. Navigate to “Project”
    >> Compute
    >> Instances
    >> Launch Instance
5. Name: wikibase1, Any availability zone, count 1
6. Source, latest ubuntu (currently 18.04)
7. Flavour: c1.large (medium should also work)
8. Key pair, create one and download the PEM :)
    >> Launch Instance . 

### Add a public IP . 

1. From the instances page, click on the actions menu on the right hand side & click Associate floating IP
2. Click the + button to add a new IP
3. Click Allocate IP && Associate

### Add firewall rules

1. Navigate to Project >> Compute >> Access & Security >> Security Groups
2. Click on “Manage Rules” for the default group
3. Allow ingress on port 22 (for ssh) for your IP (or all IPs, depending on how secure you want to be)
4. Allow ingress on the ports for wikibase & query service UI access (the defaults at 8181 and 8282) from all IPs 0.0.0.0/0 . 

### Login to the instance . 

```ssh ubuntu@111.111.111.111 # where the IP is the public IP you assigned```

### Install deps . 

```sudo apt-get update```

1. Install Docker, follow the instructions on https://docs.docker.com/install/linux/docker-ce/ubuntu/
2. Install Docker Compose, follow the instructions on https://docs.docker.com/compose/install . 

### Get your Docker Compose . 

1. In the future there will be a lovely UI that you can download a docker-compose.yml from
2. Currently you have to look at the example and work from that replacing various default values @ [Docker Compose]( https://github.com/wmde/wikibase-docker/blob/master/docker-compose.yml)

- All values that should be replaced are included next to “CONFIG” comments in the yaml
- wget https://raw.githubusercontent.com/wmde/wikibase-docker/master/docker-compose.yml # Make your modifications, or just download your modified file

```sudo docker-compose up -d #Wait for a few seconds```

```sudo docker-compose ps # check the status```

3. Upon going through this cycle, there will be an empty Wikibase ready for use. Wikibase Cloud When this screen is visible, it is ready to receive input. Given it runs on the same infrastructure as Wikidata, linked data can be added using the same data model allowing for better integration between Wikidata and the freshly installed Wikibase instance. We are now able to populate this Wikibase instance running on the Fuga Cloud server, using the same Python framework, we use to add knowledge to Wikidata.  





