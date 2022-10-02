---
title: Maintain Wikibase
description: Learn how to maintain your Wikibase.
class: maintenance
toc:
order: 5
image: /assets/images/articles/maintenance.svg
---

# Backup
The following steps create a backup of a running wikibase for future reinstall of that concents of that wikibase on the moment these commands run.

## Make a copy of the database
Backing up with mysqldump
```bash
docker exec wikibase-docker_mysql_1 mysqldump -u wikiuser -psqlpass my_wiki > backup.sql
```

For ```-psqlpass``` you should replace ```sqlpass``` with the password; for example, if the password were ```rbak9jf9adn4```, then ```-psqlpass``` would become ```-prbak9jf9adn4```. In the default example docker-compose.yml file (or the ```.env``` file) the database password ```sqlpass``` is set at two locations as ```DB_PASS``` and as ```MYSQL_PASSWORD```. The database user given for ```-u``` is set as ```DB_USER``` and as ```MYSQL_USER```. The name of the database ```my_wiki``` is set as ```DB_NAME``` and ```MYSQL_DATABASE```.

## Restore
To restore backup, either run the following command on either the existing wikibase instance, or on a new - empty - wikibase instance.

```
docker exec wikibase-docker_mysql_1 mysql -u wikiuser -psqlpass my_wiki < backup.sql
```

## Backup of blazegraph
mysqldump won't dump the contents of blazegraph, and you will have to reload, but you can also take a copy of the data.jnl file to avoid needing to do that

### Locating data.jnl
Data.jnl is created automatically and not set by the ```docker-compose.yml``` file. It can be located using the following command

```
sudo find / -iname data.jnl
```
If multiple wikibase run on the same machine, multiple locations will be reported. 
Make a copy of the relavant ```data.jnl``` file, together with the backup.sql.




