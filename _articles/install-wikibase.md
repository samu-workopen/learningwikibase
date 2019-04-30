---
title: Install Wikibase
description: Learn how to install Wikibase using Git and Composer, using the Wikibase Docker image or Open Stack
class: install-wikibase
toc:
  install-Wikibase-natively with Git and Composer: "Install Wikibase natively with Git and Composer"
  install-Wikibase-with-docker: "Install Wikibase with Docker"
  install-Wikibase-with-OpenStack: "Install Wikibase with OpenStack"
order: 1
image: iconfinder_Coding-Html_379494.svg
---

You will find different ways on how to install Wikibase. The most common are:

- [Git and Composer](https://www.mediawiki.org/wiki/Wikibase/Installation#Vagrant) 
- [Wikibase Docker image](https://github.com/wmde/wikibase-docker/blob/master/README-compose.md)
- [Openstack](https://fuga.cloud/labs/using-openstack-to-run-custom-wikibase/)

When you install Wikibase and your Wikibase Installation is public please register you installation in the [Wikibase Registry](http://wikibase-registry.wmflabs.org/wiki/Main_Page)
>
## Install Wikibase natively with Git and Composer 
>
This video will show you a screencast on how to install Wikibase natively with Git and Composer:

Video 

Prerequisite to install a Wikibase instance:
- Web server
- Git as tools (version control system) 
- composer

Prerequisite to run MediaWiki is that you have a server Media Wiki Prerequisite (apachee local server you have to remotely try out a server). There are several distributions. Best known is [Apachefriends](https://www.apachefriends.org/index.html) . 

### Clone MediaWiki repository . 

1. Set Up MediaWiki in your terminal.
2. You can clone the repository and click on the branch you want. The different versions are in the different branches: [Download from Git](https://www.mediawiki.org/wiki/Download_from_Git).
4. We tried version 1.30 to test the updates. Normally you should always use the latest version to make it stable and have the latest features. (If you took the branch away, you'd take the master, but it's currently under development). Information about current releases (newer versions of Wikibase) can be found [here](https://www.mediawiki.org/wiki/Release_notes).
5. Create a clon of the wikibase version you need with [the command](https://gerrit.wikimedia.org/r/mediawiki/core.git --branch REL1_30 mediawiki) in your command line.
6. When you create a MediaWiki Folder on your device, you can put this clone into your MediaWiki folder.  
7. As next step you can get all extensions you want to install. This is what you do with Composer. The extensions must match the MediaWiki version you downloaded. The extensions are all stored on packages. You can find them at [List of Extensions](https://gerrit.wikimedia.org/r/#/admin/projects/?filter=mediawiki%252Fextensions%252F). 
For example: when you install the vector skin by typing the following command into your command bar: composer require mediawiki/vector-skin:dev-REL1_30 → The composer automatically loads components (packages) from which the packages you install indirectly depend, e.g. the vector skin needs more packages to work. These can be found in his MediaWikiOrdnung under the "Vendor" directory. In the Composer Jason File all dependencies are defined which are needed for the project. You want to achieve that you always have the latest version.  

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

ssh ubuntu@111.111.111.111 # where the IP is the public IP you assigned . 

### Install deps . 

sudo apt-get update

1. Install Docker, follow the instructions on https://docs.docker.com/install/linux/docker-ce/ubuntu/
2. Install Docker Compose, follow the instructions on https://docs.docker.com/compose/install . 

### Get your Docker Compose . 

1. In the future there will be a lovely UI that you can download a docker-compose.yml from
2. Currently you have to look at the example and work from that replacing various default values @ [Docker Compose]( https://github.com/wmde/wikibase-docker/blob/master/docker-compose.yml)
- All values that should be replaced are included next to “CONFIG” comments in the yaml
- wget https://raw.githubusercontent.com/wmde/wikibase-docker/master/docker-compose.yml # Make your modifications, or just download your modified file
- sudo docker-compose up -d #Wait for a few seconds
- sudo docker-compose ps # check the status
3. Upon going through this cycle, there will be an empty Wikibase ready for use. Wikibase Cloud When this screen is visible, it is ready to receive input. Given it runs on the same infrastructure as Wikidata, linked data can be added using the same data model allowing for better integration between Wikidata and the freshly installed Wikibase instance. We are now able to populate this Wikibase instance running on the Fuga Cloud server, using the same Python framework, we use to add knowledge to Wikidata.  





