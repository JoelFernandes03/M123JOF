# Laborübung 1 - DNS mit Cisco Packet Tracer

Ziel dieser Laborübung ist ein *DNS Query* mithilfe des Cisco Packet Tracer zu verfolgen.

# Lernziele
 - Kennt den prinzipiellen Unterschied zwischen einer dynamischen und einer statischen Zuweisung von IP Adressen und kann aufzeigen, in welchem Anwendungsfall eine dynamische resp. eine statische Adress-Zuweisung sinnvoll ist.
 - Kennt die Einstellungen zur Konfiguration eines DHCP Servers und kann erläutern, wie diese Einstellgrössen die Zuweisung einer IP Adresse, einer Subnet-Maske und allenfalls Angaben zu DNS-Servern und Standard-Gateways bei der Anfrage eines Clients beeinflussen.

# Voraussetzung
 - *Cisco Packet Tracer*-Vorlage geöffnet (Die Vorlagen sind hier zu finden: [Vorlagen](Vorlagen/). Die Gruppeneinteilung erfolgt überlicherweise durch den Kursleiter/Leherperson.)

# Aufgabe
Die nachfolgenden Abschnitte sind Schritt-für-Schritt durchzugehen. Halten Sie alle Antworten und getätigten Konfigurationen in ihrem persönlichen Lernjournal fest (Siehe [Vorlagen](Vorlagen/)).

# Ausgangslage
![Ausgangslage](media/Ausgangslage.PNG)

Im nachfolgenden Netzwerk sind allen Geräte per DHCP eine IPv4 Adresse zugewiesen worden. Der Router *R1* agiert als DHCP Server und verteilt IP Adressen.

*Tipp: Starten Sie alle Geräte nach dem Öffnen des Labors neu. Drücken Sie dazu unten Links im *Packet Tracer* auf das nachfolgende Symbol:
![reset button cisco packet tracer](media/reset.PNG)

Anschliessend empfiehlt es sich 1 Minute zu warten, bevor mit den Aufgaben fortgefahren wird, damit alle Geräte Zeit haben per DHCP eine IPv4-Adresse zu beziehen. 

# DNS Query durchführen
Auf dem *DNS Server* sind zwei DNS Einträge konfiguriert. Ziel ist es im *Simulation*-Modus des Cisco Packet Tracer den DNS Query zu verfolgen und zu analysieren. 

1. Cisco Packet Tracer in den *Simulation*-Modus versetzen (Button unten rechts).
2. PC0 *Command Prompt* öffnen. Befehl `nslookup suche.com` eingeben. 
3. Auf der logischen Topologie erscheint ein PDU neben PDU. 
4. Doppelklick auf PDU zeigt Details an. (Achtung! Sicherstellen, dass es sich um das *DNS query* handelt!)
5. Fragen zum 

Todo:
 - Screenshot vom DNS Query mit den Details in Laborbericht einfügen. 

Fragen zum PDU *DNS Query* beantworten:
 - Wie weiss der PC0, dass er den *DNS Query* an `1.1.1.1` bzw. DNS-Server schicken muss?
 - Welches Layer 4 Protokoll kommt bei DHCP zum Einsatz?
 - Was für einen `QDCOUNT` Wert hat es und in welchem Teil ist diese Feld?
 - Wie lautet die Transaction ID?
 - Was für einen `TYPE` und `CLASS` hat der Query?

1. PDU-Fenster schliessen. PDU Weg zum Server beobachten:
2. Solange auf Next-Klicken bis das PDU den DNS-Server erreicht. 
3. Der Server generiert eine Antwort. Nun dieses PDU öffnen und folgende Fragen beantworten. 

Fragen zum PDU *DNS Query response* beantworten:
 - Was für eine IP Adresse steht in der *DNS query response* bzw. *DNS Answer*? ==> Mit Screenshot belegen. 
 - Inwiefern unterscheidet sich die *DNS query response* zur *DNS query*? In welchem/n Feld(ern) sind der "DNS-Packet-Typ" festgehalten?
