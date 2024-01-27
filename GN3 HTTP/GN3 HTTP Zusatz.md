Wenn wir beim Ping mit ping tbz.ch erfolgreich eine Antwort erhalten, dann können wir gewisse Annahmen über aktive Services treffen. Welche Voraussetzungen sind sehr wahrscheinlich erfüllt (welche Services müssen aktiv sein)?

-Der ICMP Service, DHCP DNS sind aktiv.


Was für einen User-Agent meldet der Client (Webbrowser) dem Server?(Tipp: Zu finden im HTTP Header). TCP Stream folgen mit: Rechte Maustaste auf entsprechendes Packet -> Folgen -> TCP Stream wählen.

User-Agent: Mozilla/50

Wie viele einzelne Dateien fordert der Client vom Server an?

er fordert 12 einzelne dateien ein ( nach refresh werden aber dateien gecached und möglicherweise weniger angefragt)

Was für eine Content-Encoding wählt der Webserver?

gzip encoding

Was für eine Apache Version antwortet dem Client?

Apache/2.4.56 (Debian)


uname - print system information
Um was für eine Art Unix Betriebssystem handelt es sich?

Ein Linux betriebsystem.

Welche Linux Kernelversion ist installiert?
Kernelversion 5.10.0-23-cloud-amd64.
 Ist die Kernelversion noch aktuell?
 6.8 ist die neuste version aber nicht Stable, 6.7.2 ist stable.
 
 Was für eine Prozessorarchitektur liegt vor?
  x86_64, was bedeutet, dass es sich um eine 64-Bit-Architektur handelt.
Finde ich bereits Hinweise zur Distribution?

Es ist die Debian distrubition

Output:
Linux webserver 5.10.0-23-cloud-amd64 #1 SMP Debian 5.10.179-1 (2023-05-12) x86_64 GNU/Linux

Was für eine Linux Distribution ist installiert?*

Debian

Was für IP-Adressen sind auf welchen interface konfiguriert?

lo                 UNKNOWN          127.0.0.1/8 ::1/128
ens4               UP               192.168.29.10/24 fe80::e39:72ff:fee0:0/64


Auf welchen Ports sind welche Services aktiv?
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN              10248      -
tcp6       0      0 :::80                   :::*                    LISTEN             10195      -
tcp6       0      0 :::22                   :::*                    LISTEN   

 Auf welche Ports hören bzw. antworten welche Services auf Verbindungsanfragen?
 tcp und tcp 6
 Kenne ich die Services bzw. was für Dienste stellen die jeweiligen Dienste bereit?
 
 Nein
 
Wie gross ist die Systemplatte?

etwa 2,6 GB

Steht genüngend freier Speicher zur Verfügung?

Ja steht noch 631 MB zur verfügung (in diesem Anwendungsfall)

In wie viele Partitionen wurde das System unterteilt?

2 Partionen sda1 und sda15

Filesystem      Size  Used Avail Use% Mounted on
udev            228M     0  228M   0% /dev
tmpfs            48M  344K   48M   1% /run
/dev/sda1       1.9G  1.1G  631M  64% /
tmpfs           238M     0  238M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
/dev/sda15      124M   11M  114M   9% /boot/efi
tmpfs            48M     0   48M   0% /run/user/1000


Welcher Hintergrundprozess benötigt am meisten Prozessorzeit?


    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    479 debian    20   0    8884   3428   2968 R   0.3   0.7   0:00.30 top
      1 root      20   0   98068   9700   7520 S   0.0   2.0   0:00.77 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kthreadd
      3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par+
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker+
      7 root      20   0       0      0      0 I   0.0   0.0   0:00.05 kworker+
      8 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_perc+
      9 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tas+
     10 root      20   0       0      0      0 S   0.0   0.0   0:00.00 rcu_tas+
     11 root      20   0       0      0      0 S   0.0   0.0   0:00.00 ksoftir+
     12 root      20   0       0      0      0 I   0.0   0.0   0:00.02 rcu_sch+
     13 root      rt   0       0      0      0 S   0.0   0.0   0:00.02 migrati+
     15 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0
     17 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kdevtmp+
     18 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 netns
     19 root      20   0       0      0      0 S   0.0   0.0   0:00.01 kauditd

D der top Command benutzt am meisten im moment

Steht genügend Arbeitsspeicher zur Verfügung?
mit free -h

339 MiB stehen zur verfügung sollte für unsere Aufgabe reichen.

Ziel: Konfiguration des Apache2 Webservers so passen, dass die Webseite über die Url www.daskaffee.example erreichbar ist.