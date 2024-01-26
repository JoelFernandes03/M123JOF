# GNS3 Labor - HTTP, Wireshark and hosts files

![Laboroverview](media/laboroverview.PNG)
<br>*Abbildung: Screenshot des GNS3 Labors.*

# Einleitung
![Foreversoft Solutions AG Logo](media/foreversoft-solutions-ag-logo.png)
Als Ausgangslage stellen wir uns folgendes Szenario vor: Sie sind Informatiker bei *ForeverSoft Solutions AG*, einer kleiner IT-Firma, geleitet von einem erfahrenen, aber technologisch traditionellen Chef. Die Kernkompentenz der Firma liegen in der Betreuung von Linux Servern und Netzwerktechnik für kleine KMUs. Der Chef ist eine Koryphäe in der Branche, aber die Wogen der technologischen Revolution umspülen ihn nur noch vorsichtig. Dennoch sind Techniken mit dem Label "Old School" nicht zwangsläufig Museumsstücke. "Wenn man einen Docker Container schält, findet man oft nichts anderes als eine funktionierende Mischung aus ein wenig Code, Python, Nginx oder einfach das, was die WebApplikation zum Funktionieren benötigt.", erzählt er seinen Lehrlinge gerne in der Ausbildung. 

# Lerninhalte gemäss Kompentenzmatrix
 - Konfiguration
   - C1G: Ich kann verschiedene Konfigurationsarten auflisten.
   - C1F: Ich kann bestehende Konfigurationen interpretieren und anpassen.
   - C1E: Ich kann Konfigurationen auf verschiedenen Systemen gemäss Vorbereitung erstellen. 
   - C2G: ich kann eine vorgegebene Konfiguration auf dem System umsetzen.
   - C2F: Ich kann Vorgaben eigenständig in Konfigurationen übertragen und auf dem System umsetzen.
   - C2E: Ich kann spezielle Anwendungen eigenständig in Konfigurationen übertragen und auf dem System umsetzen.
 - Installation
   - D1G: Ich kann Serverdienste/-rollen installieren.
   - D1F: Ich kenne unterschiedliche Möglichkeiten Dienste zu installieren und zu aktivieren. 
 - E1G: Ich kann feststellen, ob der Serverdienst korrekt startet.
   -  E1F: Ich kann die grundlegende Funktion eines Serverdienstes überprüfen und kenne dazu verschiedene Werkzeuge (Befehle, Schnittstellen, Log-Dateien).
   -  E1E: Ich kann Fehlfunktionen bei Serverdiensten mit einem Repertoire verschiedener Werkzeuge (Befehle, Schnittstellen, Log-Dateien) erkennen und daraus Massnahmen ableiten.

# Teil 1: Der Konfigurationsfehler
Für zwei ihrer Kunden hostet die *ForeverSoft Solutions AG* ein paar Webseiten. Die Kunden der Webseite `daskaffee.example` haben gemeldet, dass Teilweise ihre Kunden die Webseite nicht finden. Vermutet wird, dass die Kunden `www.daskaffee.example` eingeben. Dies bestätigt auch der zuständige 1st-Level-Supporter. Der für die DNS-Konfiguration zuständige Informatiker in der Firma hat bereits bestätigt, dass alle Einträge auf dem DNS Server korrekt hinterlegt sind. Sie haben nun den Auftrag erhalten, die Konfiguration des Webservers detailliert zu überprüfen. 

## Aufgabe 1: GNS3 Projekt laden
Für dieses Projekt

*GNS3 Ausgangslage*

 - Auf [mikrotik.com Account anlegen](https://mikrotik.com/client) (wenn nicht bereits vorhanden). Tipp: Account auf TBZ Adresse anlegen. 
 - Neues Projekt anlegen: Datei `M123 HTTP Labor.gns3project` in GNS3 importieren. Die Datei kann [hier](media/M123_HTTP_Labor.gns3project) herunterladen werden. 
 - *R1* starten
 - Einloggen in *R1* mit Logindaten: Username: `admin`, Passwort ist `admin`
 - Lizenz aktiveren (wird benötigt, damit der Router nicht auf 1M drosselt): `/system/license/renew`
 - Account & Passwort des MikroTik Accounts eingeben. Level `p1` wählen. 
 - Testen ob der Zugang ins Internet funktioniert: `/ping tbz.ch`

### Fragen
 - Wenn wir beim Ping mit `ping tbz.ch` erfolgreich eine Antwort erhalten, dann können wir gewisse Annahmen über aktive Services treffen. Welche Voraussetzungen sind sehr wahrscheinlich erfüllt (welche Services müssen aktiv sein)?

## Aufgabe 2: HTTP Traffic beobachten und aufzeichnen
![Wireshark intro](media/wiresharkintropicture.PNG)
<br>*Abbildung: Wireshark mit aufgezeichneten HTTP Datenverkehr. Weiter sichtbar: Aktiver Filter auf eine spezifische TCP-Verbindung, Unten rechts: Inhalt des TCP Streams.*

**Ziel: HTTP Traffic im Wireshark analysieren**

1. Im Kontextmenü der Verbindung zwischen dem Webserver und SW1 *Start capture* wählen. Im Dialog anschliessend *OK* drücken. Wireshark öffnet sich. 
![Wireshark Start Capture](media/wiresharkstartcapture.PNG)
2. Starten Sie nun den Client. Wählen dafür in dessen Kontextmenü *Start*. 
3. Konsole des Clients öffnen mit rechte Mausstaste -> *Console*.
4. Loggen Sie sich in den PC ein. Das Passwort ist gleich wie der Benutzername.
5. Öffnen Sie den Firefox (innerhalb der VM!) und besuchen sie die Webseite `daskaffee.example`. 
6. Identifizieren das erste Packet des `GET /`-Request.
7. Schauen Sie sich die Packete ein wenig genauer an indem Sie nachfolgende Fragen bearbeiten:
 - Was für einen User-Agent meldet der Client (Webbrowser) dem Server?<br>(Tipp: Zu finden im HTTP Header). TCP Stream folgen mit: Rechte Maustaste auf entsprechendes Packet -> *Folgen* -> *TCP Stream* wählen. 
 - Wie viele einzelne Dateien fordert der Client vom Server an?
 - Was für eine `Content-Encoding` wählt der Webserver?
 - Was für eine Apache Version antwortet dem Client?
8. Exportieren Sie nur die Packete ihrer HTTP Requests und speichern Sie NUR diese in ein PCAP file. 
9. **Die PCAP Datei ist dem eigenen Portfolio hinzuzufügen.**

## Aufgabe 3: Webserver - Erstinspektion
Sind auf einem Serverkonfigurationsänderungen zu machen, ist es wichtig, dass man sich zuerst einen Überblick darüber verschafft, was überhaupt für Dienste auf dem Server laufen. Nicht selten können kleine Konfigurationsänderungen ohne Vorgängiger Begutachtung der Abhängingkeiten zu grossen Komplikationen führen. 
Die nachfolgenden Befehle erlauben es sich einen Überblick über den vorliegenden Server zu machen. Zum Befehl stehen jeweils die relevanten Fragestellungen. 

Die nachfolgenden Befehle sind auf dem Webserver auszuführen und den Output jeweils zu protokollieren. Zu jedem ausgeführten Befehl sind die entsprechenden Fragen zu beantworten. 

**Ziel: Sich einen Überblick über den Webserver verschaffen insbesondere über die aktiven Dienste.**

| Befehl | Relevante Fragen |
| ------ | ---------------- |
| `uname -a` | **uname - print system information**<br>Um was für eine Art Unix Betriebssystem handelt es sich?<br>Welche Linux Kernelversion ist installiert? <br>Ist die Kernelversion noch aktuell?<br>Was für eine Prozessorarchitektur liegt vor?<br>Finde ich bereits Hinweise zur Distribution? |
| `cat /etc/issue` | **/etc/issue  is  a text file which contains a message or system identification**<br> Was für eine Linux Distribution ist installiert?* |
| `ip --brief address` | **ip - show / manipulate routing, network devices, interfaces and tunnels**<br> Was für IP-Adressen sind auf welchen interface konfiguriert? | 
| `netstat -tulpen` | **netstat -  Print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships**<br>Auf welchen Ports sind welche Services aktiv? Auf welche Ports hören bzw. antworten welche Services auf Verbindungsanfragen? <br>Kenne ich die Services bzw. was für Dienste stellen die jeweiligen Dienste bereit? | 
| `df -h` | **df - report file system disk space usage**<br>Wie gross ist die Systemplatte?<br>Steht genüngend freier Speicher zur Verfügung?<br>In wie viele Partitionen wurde das System unterteilt? | 
| `debsums --config | grep -v 'OK$'` | Dieser Befehl überprüft, welche Konfigurationsdateien verändert wurden und nicht mehr der ursprünglich installierten Version entsprechen. Der Befehl zeigt nicht an, welche Dateien das System generiert hat oder vom User hinzugefügt wurden. <br>Welche zentrale Konfigurationsdateien wurden seit der Installation verändert? |
| `top` | **top - display Linux processes**<br>Welcher Hintergrundprozess benötigt am meisten Prozessorzeit?<br>Steht genügend Arbeitsspeicher zur Verfügung? |


## Aufgabe 4: Konfiguration des HTTP Servers prüfen
*Unterstützende Ressourcen:* 
 - [DigitalOcean - How To Configure the Apache Web Server on an Ubuntu or Debian VPS](https://www.digitalocean.com/community/tutorials/how-to-configure-the-apache-web-server-on-an-ubuntu-or-debian-vps)

**Wichtig: Diese Aufgabe enthält keine Schritt-für-Schritt Anleitung, sondern ein Ziel und Tipps, wie das Ziel zu erreichen ist.**

**Ziel:** Konfiguration des Apache2 Webservers so passen, dass die Webseite über die Url `www.daskaffee.example` erreichbar ist. 

**Lösungshinweis:** In **einer einzigen** Datei muss eine Konfigurationszeile hinzugefügt werden. Eine Datei derselben Art für eine andere Webseite beinhaltet diese Zeile bereits. 

Nützliche Tools und Hinweise:
 - Apache2 Server Neustarten: `systemctl restart apache2`
 - Das Konfigurationsverzeichnis von Apache2: `/etc/apache2`
 - Alle Dateien im aktuellen Verzeichnis und Unterverzeichnissen als Baum darstellen: `tree .`
 - Inhalt einer Datei auf dem Terminal ausgeben: `cat file.txt` (file.txt durch entsprechenden Pfad ersetezen)
 - Das Zeichen `^` in den unteren zwei Zeilen des Editors `nano` stehen Für `Ctrl +` d.h. `^X` steht für `Ctrl + X`. Beim drücken dieser Tastenkombination wird der Editor geschlossen. Eine Sicherheitsabfrage bei getätigten Änderungen erfolgt. 
 - Mit dem Befehl `grep -r "stichwort"` kann im aktuellen Verzeichnis und allen Unterverzeichnissen nach einen Stichwort gesucht werden. **Achtung:** Dieser Befehl öffnet jede einzelne Datei und sucht nach dem Begriff. Es wird davon abgeraten dieser Befehl im Root-Directory `/` auszuführen!
 - Weitere wichtige Commands unter: [DigitalOcean - Top 50+ Linux Commands You MUST Know](https://www.digitalocean.com/community/tutorials/linux-commands)
 - Anzeigen welche Dateien im apache2 Folder hinzugefügt wurden: `comm -23 <(find /etc -type f | sort) <(debsums --config 2>/dev/null | awk '{print $1}' | sort) | grep "apache2"`


# Teil 2: Bye Bye Apache2 - Hallo NGINX
Der Chef ist unzufrieden mit der Performance des aktuellen Webservers. Er möchte nun von *Apache2* zu NGINX wechseln. Dazu hat er einen neuen Debian Server vorbereitet. Die IP-Adresse ist bereits konfiguriert. 

**Ziel: Die beiden Webseiten des bestehenden Webservers auf den neuen Webserver übertragen und testen.**

**Wichtig: Diese Aufgabe beinhaltet eine schrittweise Anleitung, jedoch sind nicht alle Teilschritte ausführlich erläutert. Eigeninitiative in der Recherche und eine sorgfältige Analyse der Ergebnisse sind gefordert.**

# Aufgabe 4: Migration zu NGINX auf einem neuem Testserver
1. Sicherstellen, dass der Server gestartet wurde und anschliessend über die Konsole oder SSH auf den Testserver zugreifen. 
2. Falls das Login mit dem User `debian` erfolgt ist, mit `sudo -i` *root* werden.
3. Mit dem befehl `apt update` aktuallieren wir das Repository des Paketmanagers. 
4. Mit dem Befehl `apt install nginx` installieren wir mit dem Paketmanager `apt` nginx auf unserem Server. 
5. Mit dem *change directory* Befehl in das Verzeichnis `/etc/nginx/sites-available` wechseln. In diesem Verzeichnis sind alle Webseiten konfiguriert. 
6. Mithilfe einer kurzen Internet-Recherche mit dem Inhalt `` kommen wir zum Beispiel auf die Webseite https://webdock.io/en/docs/how-guides/shared-hosting-multiple-websites/how-configure-nginx-to-serve-multiple-websites-single-vps <br>Diese Webseite erklärt uns, wie wir mehrere Webseiten auf einer NGINX-Server-Instanz konfigurieren können.
7. Nun erstellen wir eine neue Konfiguration Datei in den Ordner in den wir gerade gewechselt sind. `nano daskaffee.example.conf` mit dem Inhalt
```nginx
server {
       listen 80;
       listen [::]:80;

       server_name daskaffe.example www.daskaffee.example;

       root /var/www/daskaffee_example;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```
8. Mit dem Befehl `ln -s /etc/nginx/sites-available/daskaffee.example.conf /etc/nginx/sites-enabled/` aktivieren wir die neue Webserver-Instanz für die Domain.
9. Die Standard-Konfiguration deaktivieren mit `rm /etc/nginx/sites-enabled/default`
10. Mit dem Befehl `systemctl restart nginx` laden wir die neue Konfiguration.

Wenn wir nun mit unserem Client über den Webbrowser auf die Webseite `daskaffee.example` zugreifen, werden wir sehen, dass die Webseite angezeigt wird. Jedoch greifen wir immer noch auf den Apache2 Webserver zu. Dies können wir überprüfen, indem wir im Terminal mit dem Befehl `ping daskaffee.example` oder `nslookup daskaffee.example` prüfen in welche IPv4-Adresse die Domain aufgelöst wird.

![IPv4 Adresse wird in die falsche Domain aufgelöst](media/wrongip.PNG)
*Abbildung: Auf Ubuntu prüfen welche IP-Adresse für eine FQDN zurückgegeben wird.*

Im Abschnitt DNS haben wir gelernt, dass Windows und Linux zuerst die lokale `hosts` nach der Domain überprüfen, bevor eine DNS-Anfrage ausgeführt wird. Genau dieses Feature können wir nun ausnutzen, um zu testen ob unser WebServer funktioniert. Ein weiterer Vorteil dieser Methode ist, dass die `hosts`-Datei nur für unseren Client gilt. Andere Clients greifen können weiterhin auf den alten Webserver zugreifen, während wir den neuen Webserver einrichten. Auf dem *Client* möchten wir nun folgende Befehle im Terminal ausführen, um die Domain `daskaffee.example` Temporär auf den neuen Webserver umzuleiten.

1. Mit `sudo nano /etc/hosts` den Editor für die Datei /etc/hosts öffnen.
2. Nach den beiden *localhost*-Einträgen den Eintrag für provisorischen Eintrag einfügen, Datei abspeichern und schliessen:
![etc hosts nano editor](media/etchosts.PNG)
<br>*Abbildung: Nano Editor im Terminal mit angepasster /etc/hosts Datei*
3. Prüfen ob der neue Eintrag funktioniert mithilfe von `nslookup`:
![verify new hosts entry](media/ipverify.PNG)
<br>*Abbildung: nslookup liefert den korrekten Eintrag zurück.*


Nach getaner Arbeit greift, können wir nun endlich Testen, ob die Webseite funktioniert. Doch scheinbar ist fehlt noch etwas, denn wir bekommen die Fehlermeldung `404 Not Found` von NGINX. Scheinbar kann der Webserver die geforderte Webseite bzw. die Datei `index.html` (vgl. NGINX-Konfiguration) nicht finden. 

![404](media/404.PNG)
<br>*Abbildung: Error 404: NGINX kann zur gewünschten Domäne keine Dateien finden.*

Nebst der NGINX-Konfiguration benötigen wir natürlich auch den eigentlichen Inhalt, d.h. die Webseite. Diese finden wir auf dem alten Server und können diese per SFTP auf den neuen Server kopieren. 

Dafür führen wir den Befehl `scp -r debian@192.168.29.10:/var/www/daskaffee_example /var/www/` aus.

Anschliessend können wir im Browser die Seite aktualisieren und erhalten die Webseite angezeigt.

Auf dem Apache2-Webserver hat es nebst der Webseite `daskaffee.example` eine weitere Webseite. 

**Ziel: Alle auf dem bestehenden Apache2-Webserver vorhandenen Webseiten auf den neuen NGINX-Test-Webserver übernehmen.**

