---
title: Install Wikibase
description: Learn how to install Wikibase using Git and Composer, using the Wikibase Docker image or Open Stack
class: install-wikibase
toc:
  install-wikibase-using-the-docker-image: "Install Wikibase using the Docker image"
  install-wikibase-natively: "Install Wikibase natively"
order: 1
image:
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

Prerequisite to run MediaWiki is that you have a server Media Wiki Prerequisite (apachee local server you have to remotely try out a server). There are several distributions. Best known is [Apachefriends](https://www.apachefriends.org/index.html)


### Clone MediaWiki repository

1. Set Up MediaWiki in your terminal.
2. You can clone the repository and click on the branch you want. The different versions are in the different branches: [https://www.mediawiki.org/wiki/Download_from_Git].
4. We tried version 1.30 to test the updates. Normally you should always use the latest version to make it stable and have the latest features. (If you took the branch away, you'd take the master, but it's currently under development). Information about current releases (newer versions of Wikibase) can be found [here](https://www.mediawiki.org/wiki/Release_notes).
5. Create a clon of the wikibase version you need with [the command](https://gerrit.wikimedia.org/r/mediawiki/core.git --branch REL1_30 mediawiki) in your command line.
6. When you create a MediaWiki Folder on your Device, you can put this clone into your MediaWiki folder.  
7. next you get all extensions you want to install. This is what you do with Composer 
The extensions must match the MediaWiki version you downloaded. The extensions must be in the same version. The extensions are all stored on packages (these are all packages that can be loaded with Composer (Wikimedia has stored them all there). 
For example, you install the vector skin by typing the following command into your command bar: composer require mediawiki/vector-skin:dev-REL1_30 → The composer automatically loads components (packages) from which the packages you install indirectly depend, e.g. the vector skin needs more packages to work. These can be found in his MediaWikiOrdnung under the "Vendor" directory. In the Composer Jason File all dependencies are defined which are needed for the project. You want to achieve that you always have the latest version.

### Add extensionsion using composer
  
The whole extensions can also be cloned without the composer, but with the composer it is easier.
2. now you have to install all the dependencies of Media Wiki yourself e.g. Debugging
3. Now you have your web server. You can now install MediaWiki. (You have two paths (the local web server directly on the computer and the web directory, which is external) → you access the directory you have cloned MediaWiki into via your browser.
Then you execute the MediaWiki installation routine → You are led through a menu. 
5. then you can access your wiki. Now you can see that the vector skin (extension) is already inside automatically. 

### Configure Wikibase

1. if you click on "Special Version" → you need to activate Wikibase Language Version (you need this to create labels in different languages) and Wikibase Repro (you need this to create items and properties on your Wikibase instance) → is also located in the Docker Image
2. what you have to do is activate the local settings. To do this you have to copy the LoW Extension command into the Local Settings PHP version (the file will be generated when you finish the MediaWiki installation, then copy it to your main directory). The command is: wfLoadExtension( 'UniversalLanguageSelector' ); 
The activation of Wikibase is done from the Wikibase installation page: https://www.mediawiki.org/wiki/Wikibase/Installation e.g. you can only activate the repro if only this is needed e.g. depending on the purpose (e.g. you can decide if the client is needed or not)
4. if you've updated your special version now, you're ready to go. If you use this command: php maintenance/update.php
5. executes all basic things will be created (e.g. the database tables you need to work in Wikibase)
6. the wikibase repro you have installed should work this way. 


## Install Wikibase with a Docker Image 

This Video will show you a Docker Image Installation: 
VIDEO

All informations about the Docker installation you can find here:https://github.com/wmde/wikibase-docker/blob/master/README-compose.md

## Open Stack to run a custom Wikibase

