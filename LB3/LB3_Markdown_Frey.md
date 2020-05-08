# Markdown Dokumentation - M300 Plattformübergreifende Dienste in ein Netzwerk integrieren - LB3

## **Übersicht**

* Beschriebung
* Docker
* Bilder
* Ergebnis
## **Beschreibung**
***
### [Apache](https://httpd.apache.org//) - Webserver 
Quelloffener Webanbieter. Apache ist gratis und kann/ darf von jedem verwendet werden. Apache ist grundsätlich für jedes Betriebssystem erhältich. Apache HTTP Server wurde 1995 das erste mal veröffentlicht.

## **Docker**
***
Herr Berger hat uns bereits die Nötigsten Dokumente *(Vagrant File + mehrere Standardservices)*. Die Video Pitches waren sehr hilfreich und haben meine Fragen beantwortet.
```
vagrant up
```
Damit wir Docker verwenden können müssen wir zuerst eine VM erstellen. In meinem Fall hat das ein Vagrant File aus Ihrem Repository für mich erledigt. 

In Meinem Fall wäre das [Ubunto 16.04 Xenial](https://releases.ubuntu.com/16.04/)
Mit folgendem Befehl kann die System Version bzw. Betriebssystemversion angezeigt werden
```
lsb_release -a
```
Nach erstellung der VM, kann man mit folgendem Befehl direkt auf die VM zugreifen/ verbinden.
```
vagrant ssh
```
Ich habe mich für einen Apache Webserver entschieden. Nach Eingabe des untenstehenden Befehls, wechselnn Sie in das Apache Verzeichnis.
```
cd /vagrant/apache
```
Wir sind zwar nun im Verzeichnis müssen aber den Docker Apache Webserver noch starten lassen. Das passeirt mit folgendem Befehl. **WICHTIG** Punkt am Schluss nicht vergessen.
```
docker build -t apache .
```
Auch hier müssen wir eine **Portweiterleitung** einrichten. In meinem Fall wird der Befehl gleich mit dem starten eines Containers zusammengefügt. Wir leiten den Port 80 auf 8080 weiter.
```
docker run --rm -d -p 8080:80 -v `pwd`/web:/var/www/html --name apache apache
```
## **Bilder**
***
[Bilder](https://imgur.com/gallery/ByIqeBK)

Zum Schluss habe ich noch getestet ob die Standart HTML-Seite aufrufbar ist. Zum Glück hat alles so einwandfrei funkioniert und ich hatte im Vergleich zu Vagrant viel weniger Fehler und Unterbrechungen.

[Bilder](https://imgur.com/gallery/x0JwOcK)
Hier sehen Sie den Netzadapter/ IP-Adresse welche in diesem Fall für PUTTY verwendet wurde. Lässt sich mit folgendem Befehl auslesen.
```
ifconfig -a
```
## **Ergebnis**
***
Es wurde Erfolgreich eine Docker Umgebung erschaffen mithilfe eines Vagrantfiles. Der darauf laufende Service heisst Apache und funktioniert einwandfrei. Es können eigene Webseiten gehostet werden. Die Internetseiten lassen sich selbstverständlich aufrufen. Momentan ist noch die Standart HTML-Seite verknüpft. Das passiert mit einer kleinen Portweiterleitung.
Mithilfe von SSH bzw. PUTTY kann man auf die VM zugreifen und diese per CLI steuern.

###### *Vito Frey - M300 Plattformübergreifende Dienste in ein Netzwerk integrieren LB3* 
