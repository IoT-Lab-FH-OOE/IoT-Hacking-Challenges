$$\Large{\color{red}Spoiler \space ahead!}$$

> __Warning__
Nachfolgend ist die gesamte Dokumentation der Challenge 1. Dies inkludiert auch Konfigurationen bzw. Zugangsdaten, die im Laufe der Challenge ausfindig zu machen sind.
Nicht weiterlesen, wenn du diese Challenge noch absolvieren möchtest!

<details>
<summary>Dokumentation der Implementierung anzeigen...</summary>
<br>

Die Challenge kann ganz einfach mit dem Script [install_exploit.sh](Challenge1_exploit/install_exploit.sh) installiert werden. Dieses fügt zuerst ein [Service](Challenge1_exploit/challenge1-exploit.service) zu systemctl hinzu. Anschließend wird ein Cronjob erstellt, welcher bei jedem Start des Systems das Script [start_exploit.sh](Challenge1_exploit/start_exploit.sh) ausführt. Dieses Script nimmt ein paar Netzwerkeinstellungen vor und startet das zuvor hinzugefügte Service (challenge1-exploit). Das Service ruft den eigentlichen Exploit bzw. das Python Script, welches den Controller simuliert und den Button Input verarbeitet, auf.


### Konfiguration des Raspberry Pi

**Betriebssystem:** Kali 2022.4

**Zugangsdaten:**<br>
User: kali<br>
Password: kali<br>
<br>
An den Raspberry Pi wird per USB der WLAN-Adapter angeschlossen, der USB-Port kann frei gewählt werden, der Raspberry erkennt den Adapter automatisch und bindet ihn als wlan1 Interface ein. Beim Start des Betriebssystems startet automatisch das Drohnen Controller Simulator Programm. Dieses sucht über das wlan1 Interface ein WLAN das mit der SSID Drone- beginnt und verbindet sich zu diesem und gibt sich selbst die IP-Adresse 172.50.10.254. Anschließend schickt es periodisch folgende Kommandos an die Drohne (172.50.10.1):<br>
Modus 1:<br>
1 Sekunde Start motor commands (10 Nachrichten)<br>
10 Sekunden Keep alive commands (10 Nachrichten)<br>
1 Sekunde Stop motor commands (10 Nachrichten)<br>
180 Sekunden Keep alive commands (180 Nachrichten)<br>
Modus 2:<br>
Start motor commands alle 1 Sekunde

</details>
