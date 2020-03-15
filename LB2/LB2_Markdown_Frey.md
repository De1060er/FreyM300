# Markdown Dokumentation - M300 Plattformübergreifende Dienste in ein Netzwerk integrieren

## **Übersicht**

* Beschriebung
* Vagrantfile
* Bilder
* Ergebnis
## **Beschreibung**
***
### [Nginx](https://www.nginx.com/) - Webserver 
Perfomanter Webserver im bereich Linux/ Ubuntu. Sehr minimale 
Systemanforderungen. Nginx ermöglicht es eigene Webseiten zu hosten. 
Multiple-Site Hosting mit nur einem Nginx-Server möglich.

## **Vagrantfile**
***
Damit Vagrant weiss wo der Befehl Block beginnt muss folgender Befehl gesetzt sein
```
Vagrant.configure(2) do |config|
```
Vagrant muss wissen mit welche Box bzw. OS es installieren soll.

In Meinem Fall wäre das [Ubunto 18.04 Bionic Reaver](https://app.vagrantup.com/ubuntu/boxes/bionic64)
```
config.vm.box = "ubuntu/bionic64"
```
Nun müssen wir noch eine **Portweiterleitung** einrichten, so können wir unsere Maschine von aussen erreichen. Ich habe mich für den Port **7999** entschieden da dieser Frei war.
```
config.vm.network "forwarded_port", guest:80, host:7999, auto_correct: true
```
Manachmal macht es Sinn einen **Gemeinsamen Ordner** festzulegen. So können Dateien welche Lokal auf dem PC gespeichert sind auf die VM übertragen werden. Dateien die in dem Ordner landen sind in der VM/ Ubuntu Webserver sichtbar.
```
#config.vm.synced_folder ".", "/var/www/html"
```
*Optionaler Befehl*
Die VM erhält so eine IP-Adresse *(192.168.0.1)*. In meinem Fall wäre das nicht unbedingt nötig gewesen, aber ich wollte schauen ob es ohne Fehler funktioniert.
```
config.vm.network :private_network, ip: "192.168.0.1"
```
## VirtualBox
Wir können der VM noch ein paar Parameter mitgeben bei der Installation. Es ist wichtig für Vagrant einen neuen Kommando Block zu starten. Für mich war wichtig das die VM einen *"normalen"* Namen erhält. Standardmässig werden die Namen sehr lang. Neben dem habe ich ihr mehr CPU's und RAM zur Verfügung gestellt.
```
config.vm.provider "virtualbox" do |vb|
  vb.name = "Nging_Webserver"
  vb.cpus = "2"
  vb.memory = "1024"
end
```
### *Hinweis*
Vagrant selber erlaubt es Kommando direkt im File mitzugeben. Ich habe mich aber dazu entschieden ein extra Skript zu erstellen. Dort wird der **Install Nginx** Befehl mitgegeben.

```
config.vm.provision "shell", path: "shells.sh"
```
Sucht nach Updates und installiert/ startet Nginx Service
```
apt-get -y update

apt-get -y install nginx
 
service nginx start
```
###  ***Achtung!***
Es ist sehr wichtig die einzelnen Kommando Blocks mit dem end befehl zu schliesen. Man kann es mit einer PowerShell-Schleife vergleichen welche kein Ende gesetzt bekommen hat. --> Sehr schlecht
```
end
```
## **Bilder**
***
![Ergebnis](https://github.com/De1060er/FreyM300/tree/master/Nginx2/Bilder/Welcome_Nginx.png)
Nach der Installation sollte unser Webserver ansprechbar sein. Wir können nun einen Internet-Browser öffnen und mit **LOCALHOST:7999** sollte sich diese Seite öffnen.

### **Ausbau**
Nun ist der Webserver einsatzbereit und sollte bereit sein neue Seiten zu hosten. Ich habe mir überlegt die Standard-HTML Seite zu ändern, natürlich sollte dies mit einem Script passieren und nicht von Hand.

Es hat sich herausgestellt das in Nginx die einzelnen Seiten zu stark verlinkt sind und es mehrere Config-Dateien gibt. Nach längerem herumprobieren hat es dan für ein paar mal geklappt die Index.html seite zu ändern und diese auch wieder aufzurufen im Browser. Teilweise hat vagrant beim aufsezten ein bisschen Probleme verursacht und es kam zu Errors. *(Siehe Bild unten - Eigentliche neue Seite )*
![Ergebnis](https://github.com/De1060er/FreyM300/blob/master/Nginx2/Bilder/Welcome.PNG)

In einem Weiterem Skript wollte ich die Standart Index.html Seite, welche von Nginx mitgegeben wird ändern. Mit Folgendem Befehl sollte die Index.html Seite überschrieben werden mt einem anderem Text und anderer Farbe. Danach musste der Nginx-Service neugestartet werden. Erst dann hätten die Änderungen ersichtlich sein können.
```html
echo "<html>
<head>
<title>Nginx Webserver</title>
</head>
<body bgcolor="black" text="white">
<center><h1>Nginx wurde Erfolgreich Installiert!</h1></center>
</body>
</html>" > /usr/share/nginx/html/index.html

sudo nginx -t
```

## **Ergebnis**
***
Das Vagrantfile erstellt erfolgreich eine VM mit dem Ubuntu-OS *(Bionic Reaver 18.04)*. Nachdem die VM aufgesetzt wurde wird automatisch der Nginx Webdienst heruntergeladen & installiert. Nginx ermöglicht ein Multiple-Webhosting das bedeutet es könnten meherer Internetseiten gleichzeitig Angeboten/ Betrieben werden. In meinem Fall wird unter der Adresse **Localhost:(Port = 7999)** die Standart Welcome Internetseite von Nginx angezeigt.

###### *Vito Frey - M300 Plattformübergreifende Dienste in ein Netzwerk integrieren* 
