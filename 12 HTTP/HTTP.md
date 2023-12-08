P A
m123
Repository
Philipp Albrecht's avatar
improvements http Aufgabe
Philipp Albrecht authored 1 month ago
63d14a90
m123
07_HTTP
HTTP_Webserver
Name	Last commit	Last update
..
media	improvements http Aufgabe
1 month ago
README.md	improvements http Aufgabe
1 month ago
dummy-website.zip LFS	added beginner friendly http experiment
1 month ago
 README.md
GNS3 Labor - HTTP, Wireshark and hosts files
Einleitung
In dieser Übung setzt du auf einer virtuellen Maschine (VM) in der Cloud einen Webserver auf und beobachtest den Datenverkehr zwischen deinem Webserver und deinem Laptop.

Lerninhalte gemäss Kompentenzmatrix
Konfiguration
C1G: Ich kann verschiedene Konfigurationsarten auflisten.
C1F: Ich kann bestehende Konfigurationen interpretieren und anpassen.
C1E: Ich kann Konfigurationen auf verschiedenen Systemen gemäss Vorbereitung erstellen.
C2G: ich kann eine vorgegebene Konfiguration auf dem System umsetzen.
C2F: Ich kann Vorgaben eigenständig in Konfigurationen übertragen und auf dem System umsetzen.
C2E: Ich kann spezielle Anwendungen eigenständig in Konfigurationen übertragen und auf dem System umsetzen.
Installation
D1G: Ich kann Serverdienste/-rollen installieren.
D1F: Ich kenne unterschiedliche Möglichkeiten Dienste zu installieren und zu aktivieren.
E1G: Ich kann feststellen, ob der Serverdienst korrekt startet.
E1F: Ich kann die grundlegende Funktion eines Serverdienstes überprüfen und kenne dazu verschiedene Werkzeuge (Befehle, Schnittstellen, Log-Dateien).
E1E: Ich kann Fehlfunktionen bei Serverdiensten mit einem Repertoire verschiedener Werkzeuge (Befehle, Schnittstellen, Log-Dateien) erkennen und daraus Massnahmen ableiten.
Vorbereitungen
Hole dir bei der Lehrperson die Zugangsdaten für eine VM ab. Die VM wird im Moment erstellt und ist bis am Ende des Tages verfügbar.

Wichtig: Aus technischen Gründen kann es vorkommen, dass die VM bereits früher ausgeschaltet wird. Es ist deshalb wichtig, dass du deine Arbeit dokumentierst (Ausgeführte Befehle, getätigte Konfigurationen, usw. )

Teil 1: Login per SSH
SSH Login Prompt in Windows Terminal
Abbildung 1: Windows Commmand Propmt, SSH Client fordert zur Eingabe des Passwortes auf

In Windows, macOS und die meisten Linux Distributionen ist SSH Client Programm integriert. Öffne die Konsole (Unter Windows cmd.exe). Verwende den nachfolgenden Befehl, um dich mit deiner VM zu verbinden. Ersetze IP durch die IP-Adresse des Servers und user durch den entsprechenden Usernamen. Die Angaben solltest du von der Lehrperson erhalten haben.

ssh user@IP
Nachfolgend wirst du aufgefordert das Passwort einzugeben.

Während der Eingabe siehst du auf dem Terminal kein echo (In einem Terminal bedeutet "Echo", dass die Eingaben des Benutzers (wie Tastenanschläge) auf dem Bildschirm sichtbar wiedergegeben werden.)

Wenn die SSH Verbindung erfolgreich aufgebaut ist, siehst du die Befehlzeile des Servers

azureuser@mmuster:~$
Wenn du via SSH auf einem anderen System eingeloggt bist, bedeutet das, dass eine Verbindung über das Internet oder lokale Netzwerk eine verschlüsselte Verbindung zu einem anderen Computer hergestellt wurde. Es wäre, als sässen sie direkt vor dem Comptuer, allerdings ohne die gewohnte grafische Oberfläche wie Ihren Desktop, sondern über das CLI (Command Line Interface).

Teil 2: Navigation in der CLI
Im Linux/Unix-Betriebssystem ist alles eine Datei, sogar Verzeichnisse sind Dateien, Dateien sind Dateien und Geräte wie Maus, Tastatur, Drucker usw. sind ebenfalls Dateien. Hier werden wir die Verzeichnisstruktur in Linux betrachten.

Alles über die Grundlagen von Linux, insbesondere darüber, wie du dich in der Kommandozeile zurechtfinden kannst, findest du in zahlreichen Ressourcen im Internet.

Ein paar Ressourcen:

https://linuxjourney.com/
https://www.geeksforgeeks.org/linux-directory-structure/
https://www.terminaltutor.com
Teil 3: Services
Als erstes möchten wir prüfen, welche Services, die über das Netzwerk verfügbar sind, auf dem Server aktiv sind. Dafür verwenden wir den Befehl sudo netstat -tulpen.

netstat -tulpen
Abbildung 2: Ausgabe des Befehles netstat

Der Befehl gibt eine Tabelle aus. Auf jeder Zeile ist ein Service beschrieben. Unter Local Address sehen wir auf welcher IP-Adresse und welchem Port die Anwendung auf neue Pakete lauscht.

127.0.0.53:53 bedeutet, dass der die Anwendung auf der loopback Adresse 127.0.0.53 auf dem Port 53 lauscht. Aus der IP-Adresse können wir schliessen, welche Hosts mit dem Service kommunizieren können. Services, welche auf eine Loopback-Adresse lauschen, sind nur von vom Host selber erreichbar.

Bei 0.0.0.0:22 hört der Service auf dem Port 22. 0.0.0.0 bedeutet, dass der Service auf allen Netzwerkinterfaces des Computers lauscht. Am Ende der Zeile steht, welche Applikation auf dem Port lauscht: 1488/sshd. Das steht für Prozess mit der PID (Prozess ID) 1488 mit dem Namen sshd (ssh Daemon). Dieser Service ist das gegenstück zu unserem SSH Client und erlaubt es, dass SSH Clients eine SSH Verbindung zum Server aufbauen. Wenn der SSH Daemon nicht läuft, steht der SSH Service nicht zur Verfügung und SSH Clients können keine Verbindung aufbauen.

Teil 4: Installation Webserver
Mithilfe des Paketmanagers apt können wir einen Webserver installieren. Ein beliebte Implementation eines Webservers ist NGINX. Mit

sudo apt install nginx
Der Befehl lädt alle erforderlichen Komponenten herunter, installiert diese und startet anschliessend den Webserver mit der Standardkonfiguration.

Durch ein erneutes ausführen des Befehl sudo netstat -tulpen kann festgestellt werden, dass nun ein weiterer Service aktiv ist:

netstat -tulpen with nginx
Abbildung 3: Ausgabe des Befehles netstat. Im Vergleich mit einem neuen aktiven Service.

tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      0          39997      2989/nginx: master

NGINX mit der IPD 2989 wartet auf dem Port 80 auf TCP Verbindungsanfragen.

Mit dem Webbrowser (ein HTTP Client, das Gegenstück zum Webserver) können wir nun auf die Seite zugreifen:

Welcome NGINX Screenshot
Abbildung 4: Webbrowser mit dem Inhalt der NGINX Standard Webseite

Teil 5: Webseite modifizieren
Eine Standardwebseite bringt uns noch nicht sehr viel. Natürlich möchten wir unseren eigenen Content bereitstellen oder unsere eigene Applikation laufen lassen.

Beginnen wir den Basics und notifieren das HTML der aktuellen Welcome Seite nach unseren Bedürfnissen.

Wechseln in das Verzeichnis, dass der Webserver via HTTP zur Verfügung stellt:

$ cd /var/www/html
Mit ls (list directory content) können wir Anzeigen was für Dateien, Ordner, Links, usw. in diesem Ordner sind.

$ ls
index.nginx-debian.html
Bearbeiten wir die Datei mit einem Texteditor:

$ sudo nano index.nginx-debian.html
Mit den Pfeiltasten können wir uns nun im Source Code der HTML Datei navigieren.

Beispiel: Titel der Seite bearbeiten: Den bestehenden Titel... nano index.html

...durch Hello World ersetzen nano hello world
Abbildung 5: nano editor mit geöffneter index.nginx-debian.html Datei.

Sobald die Datei gespeichert ist (Ctrl+O), kann die Änderung über den Webbrowser eingesehen werden (Seite aktuallisieren, z.B. mit F5).

Hello World im Webbrowser
Abbildung 6: Die geänderte Zeile in nano Texteditor.

Teil 6: HTTP Datenverkehr beobachten
Mithilfe von Wireshark kann der Datenverkehr zwischen dem Server und dem eigenen Laptop beobachtet werden.

Wireshark kann auf https://www.wireshark.org/ heruntergeladen werden.

Ziel: Die Übertragung der Webseite mit Wireshark abfangen.

Wireshark SSH
Abbildung 7: Wireshark mit diversen Paketen

Sobald wir den packet caputre gestartet haben, sehen wir jede Menge Datenpakete, die der eigene Laptop mit anderen Hosts (vor allem Server im Internet) austauscht. Um die relevanten Pakete herauszufiltern empfiehlt es sich deshalb den Filter zu verwerden. Das Ziel ist es den HTTP Verkehr zwischen dem eigenen Laptop und dem HTTP Server mitzuschneiden. Am einfachsten geht das, wenn nach der Server IP und dem Destination port gefiltert wird.

In der Textbox mit dem Inhalt Apply a display filter ist zu hinterlegen:

ip.addr == 203.0.113.41 && tcp.port == 80
Wichtig: Die IP-Adresse (hier 203.0.113.41) ist mit der des eigenen Servers zu ersetzen!

Sobald die Webseite im Browser (neu) geladen wird, sollten im Trace Pakete auftauchen.

Mit der Funktion Follow kann der Inhalt des Datenverkehrs zwischen dem Server und dem Host betrachtet werden. (Rechte Maustaste auf das erste relevante HTTP Packet (erkennbar an der Info GET / HTTP/1.1), Follow => TCP Stream).

Wireshark HTTP TCP follow
Abbildung 8: Wireshark. Im Hintergrund: Auflistung der HTTP Pakete, Im Vordergrund: Inhalt des Datenverkehrs zwischen dem Client und dem Host.

Teil 7: Eigene Webseite hochladen
Die Erstellung einer eigenen Webseite oder Webapplikation ist ein sehr umfangreiches Thema für sich. Es gibt unzählige Arten und Möglichkeiten das zu realisieren. In diesem Modul möchten wir eine statische Webseite auf unseren Webserver laden. Dazu gibt es verschiedene Möglichkeiten.

Im Ordner zu dieser Datei ist eine Testwebseite als ZIP Datei abgelegt (dummy-website.zip).

Lade diese Datei auf dein persönliches Notebook auf den Desktop herunter.

Mithilfe von SSH können wir uns nicht nur mit der Befehlszeile des Servers verbinden, sondern auch Datei und Ordner hoch und herunterladen.

Herausforderung:

Versuche nun selbstständig herauszufinden, wie du die die Webseite auf deinen Webserver laden kannst. Ziel ist es, dass der Inhalt der Zipdatei (der eigentliche Inhalt der Webseite) über den Webbrowser erreichbar ist.

Notiere die einzelnen Schritte und Befehle. Sobald du es geschafft hast, bespreche deinen Lösungsweg mit der Lehrperson.

Rechtliche Hinweise
Über den Server dürfen keine urheberrechtlich geschützten Materialien verbreitet werden. Der Upload von Inhalten, die gegen die Menschenwürde, gesetzliche Bestimmungen, Gewalt verherrlichen oder Hass fördern, ist untersagt. Zudem ist das Teilen von Inhalten, die allgemein als unangemessen oder beleidigend gelten, nicht erlaubt.

Der Server darf nur für Übungen im Rahmen des Moduls 123 genutzt werden. Lernende, die darüber hinausgehende Experimente auf dem Server durchführen möchten, müssen dies vorab mit der Lehrperson abklären.