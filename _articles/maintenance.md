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

-psqlpass and -u are set in the docker-compose file that is used to run the wikibase. In the default example docker-compose.yml file the -psqlpass is set at two locations. as ```DB_PASS``` and as ```MYSQL_PASSWORD```. ```-u``` is set as ```DB_USER``` and as ```MYSQL_USER```

# Restore
To restore backup, either run the following command on either the existing wikibase instance, or on a new - empty - wikibase instance.

```
docker exec wikibase-docker_mysql_1 mysql -u wikiuser -psqlpass my_wiki < backup.sql
```



