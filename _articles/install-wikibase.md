---
title: Install Wikibase
description: Wikibase may be installed using Git and Composer, or using the Wikibase Docker image.
class: install-wikibase
toc:
  install-wikibase-using-the-docker-image: "Install Wikibase using the Docker image"
  install-wikibase-natively: "Install Wikibase natively"
order: 1
image:installing.html
---

## Install Wikibase using the Docker image

- Openstack
- https://fuga.cloud/labs/using-openstack-to-run-custom-wikibase/
- Dataarchitecture
- Dataimport 
- Dataexport
- Visualisation 
- Maintanance 
- USE Cases 

## Install Wikibase natively

Vorraussetzung, um eine Wikibase Instanz zu installieren:
- Webserver
- Git als Tools (Versionsverwaltungssystem) 
- Composer

Vorraussetzung MediaWiki laufen zu lassen ist, dass man einen Server hat Media Wiki Vorraussetzung (apachee lokaler Server, den man hat, um remote einen Server auszuprobieren). Es gibt verschiedene Distributionen. Am bekanntesten ist: https://www.apachefriends.org/index.html

### Clone MediaWiki repository

1. MediaWiki wird aufgesetzt über den Befehl 
2. Das Repository kann man klonen und den Branch anklicken, den man möchte. Die verschiedenen Versionen sind in den verschiedenen Branch’s: https://www.mediawiki.org/wiki/Download_from_Git
4.  → Henning hat Version 1.30 ausprobiert, um die Updates zu testen. Normalerweise sollte man immer die neuste Version nehmen, damit es stabil ist und die neusten Features hat.
(Wenn man die Branch wegnehmen würde, würde man den Master nehmen, aber auf der wird aktuell entwickelt). Informationen über aktuelle Releases (neuere Versionen von Wikibase) finden sich hier:https://www.mediawiki.org/wiki/Release_notes
5. Einen Clon der Wikibase Version, die man benötigt erstellt man durch den Befehl: https://gerrit.wikimedia.org/r/mediawiki/core.git --branch REL1_30 mediawiki in seiner Comandoleiste
6. Diesen Clon schiebt man in seinen MediaWiki Ordner 
7. Als nächstes holt man sich alle Extensions, die man installieren möchte. Das macht man mit Composer 
Die Extensions müssen zu der MediaWiki Version passen, die man runtergeladen hat. Die Extensions müssen in der gleichen Version sein. Die Extensions sind alle auf Packages (das sind alle PAckages, die man mit Composer laden kann (Wikimedia hat dort alle abgelegt) abgelegt. 
8. Beispielsweise installiert man den Vektor Skin indem man folgenden Befehl in seine Kommandoleiste eingibt: composer require mediawiki/vector-skin:dev-REL1_30 → Der Composer lädt automatisch Komponenten (Packages) von den das Packages, dass man indirekt installiert, abhängig ist z.B. der Vektor Skin benötigt weitere Packages, um zu funktionieren. Diese findet man in seinem MediaWikiOrdnung unter dem “Vendor” Verzeichnis. Im Composer Jason File sind alle Abhängigkeiten definiert, die man für das Projekt benötigt. Man will dadurch erreichen, dass man immer die aktuelle Version hat.

### Add extensionsion using composer
  
1. Die ganzen Extensions kann man auch so clonen ohne den Composer, mit diesem ist es jedoch einfacher.
2. Nun muss man noch die ganzen Abhängigkeiten von Media Wiki selber installieren z.B. Debugging
3. Jetzt hat man seinen Webserver. Man kann jetzt MediaWiki installieren. (Man hat zwei Pfade (den lokalen Webserver direkt auf dem Computer und das Webverzeichniss, welches extern ist) → man greift über den Browser auf das Verzeichnis zu, in das man MediaWiki reingeklont hat.
4. Dann führt man die MediaWiki Installations Routine aus → Durch ein Menü wird man hindurchgeführt. 
5. Dann kann auf sein Wiki zugreifen. Jetzt sieht man, dass der Vektor Skin (Extension) schon automatisch drinn ist. 

### Configure Wikibase

1. Wenn man “Special Version” anklickt → muss man noch Wikibase Language Version (das braucht man, um in verschiedenen Sprachen Labels anlegen zu können) und Wikibase Repro (das will man haben, um Items und Properties erstellen zu können auf seiner Wikibase Instanz) aktivieren → befindet sich auch im Docker Image
2. Was man machen muss, ist die Local Settings aktivieren. Dazu muss man die LoW Extension Anweisung in die Local Settings PHP Version (die Datei wird generiert, wenn man die MediaWiki Installation abschließt, die kopiert man dann in sein Hauptverzeichnis). Der Befehl ist: wfLoadExtension( 'UniversalLanguageSelector' ); 
3. Die Aktivierung von Wikibase nimmt man von der Wikibase Installationsseite: https://www.mediawiki.org/wiki/Wikibase/Installation z.B. kann man nur das Repro aktivieren, wenn nur dieses braucht ist z.B. Abhängig vom Gebrauchszweck ( z.b. Kann man sich überlegen, ob der Client benötigt wird oder nicht)
4. Wenn man jetzt seine Special Version aktualisiert ist, kann man loslegen. Wenn man dieses Comando: php maintenance/update.php
5.  ausführt werden alle Grundsachen angelegt (z.B. die Datenbanktabellen, die man braucht, um in Wikibase zu arbeiten)
6. Das Wikibase Repro, was man installiert hat, sollte so grundsätzlich funktionieren. 
