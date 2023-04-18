$$\Large{\color{red}Spoiler \space ahead!}$$

> __Warning__
Nachfolgend ist die Musterlösung der Challenge 2.
Nicht weiterlesen, wenn du diese Challenge noch absolvieren möchtest! Hinweise zum Lösen der Challenge findest du am Ende der Angabe.

<details>
<summary>Musterlösung anzeigen...</summary>
<br>

### Zerlegen des Routers

Da es Ziel ist auf die UART-Schnittstelle des Netgear Routers zuzugreifen, muss dieser geöffnet werden. Dazu entfernt man zuerst auf der Unterseite alle 5 Schrauben (Dies sollte im SESAM bereits erledigt sein).<br>
Nun kann man den Deckel abnehmen und legt die Platine des Routers frei. Diese kann im restlichen Gehäuse gelassen werden und muss nicht ganz herausgenommen werden.

<img src="resources/Router.jpg" width="300">

### Finden der UART-Schnittstelle

Um die UART-Schnittstelle zu finden, wird ein Logic Analyzer von SALEA verwendet.<br>
Bei den meisten Geräten ist die UART-Schnittstelle auf Debugging-Pins wie hier zu finden. Leider sind sie oft nicht beschriftet oder versteckt.

<img src="resources/UART-Pins.jpg" width="300">

Um die UART-Pins richtig zuzuordnen, verbindet man die 4 Pins mit dem Logic Analyzer und sucht nach Daten, die übertragen werden. Nicht vergessen sollte man hier eine zusätzliche Verbindung zur Masse des Routers. Im Fall dieses Routers werden Bootloader- und Kernel-Protokolle beim Start auf diese Schnittstelle ausgegeben, so dass man diese mitlesen kann, sobald man die richtigen Pins gefunden hat.

<img src="resources/Logic_Analyzer_Connect.jpg" width="300">

In der Software des Logic Analyzers stellt man die digitalen Channel 0-4 ein und deaktiviert alle analogen Channel. Eine Abtastrate von mindestens 8MS/s sollte gewählt werden.

<img src="resources/Logic_1.png" width="300">

Startet man nun die Aufnahme und schaltet den Router ein, so sieht man bei 2 Channel eine Veränderung. Channel 3 ist stetig auf High, was bedeutet hier liegen einfach nur 3,3V an. An Channel 1 sieht man viele Sprünge von 0 auf 3,3V und zurück, was auf übertragene Daten hinweist.

<img src="resources/Logic_3.png" width="300">

Nun wählt man aus der Liste der Analyzer den Async Serial aus, ein Analyzer, der simplen Seriellen Traffic lesen kann. Als Input-Channel wählt man hier den Channel mit den potentiellen Daten, in diesem Beispiel Channel 1. Alle anderen Parameter lässt man fürs erste auf den Standard-Einstellungen, da diese die meist verbreitetsten sind und die Wahrscheinlich mit diesen ein Ergebnis zu erzielen sehr hoch ist.

<img src="resources/Logic_4.png" width="300">

Schaut man sich nun den Konsolen-Output des Analyzers an, erkennt man bereits lesbaren Text. Dies ist Ausgabe-Text des Routers beim booten, was bedeutet die UART-Schnittstelle wurde gefunden.

<img src="resources/Logic_5.png" width="300">

Channel 0 und 2 sind Ground und der Pin für Daten Input, also Konsolen Befehle. Prüft man mit einem Multimeter nun die Pins gegen Masse, erkennt man, dass Channel 2 ebenfalls Masse ist. Dies bedeutet die Pins sind von Links nach rechts:

| Pin1 | Pin2| Pin3| Pin4|
|---|---|---|---|
| 3.3V | GND | TX | RX |

### Herstellen der Verbindung

Um nun eine Verbindung zwischen Computer und Konsole des Routers herzustellen, wird ein USB zu TTL Adapter verwendet.<br>
Man verbindet GND mit GND, RX des Adapters mit TX des Routers, und TX des Adapters mit RX des Routers. Der Adapter wird dann über USB mit dem Computer verbunden.
  
<img src="resources/Adapter.jpg" width="300">
  
Man verwendet nun ein geeignetes Programm, um mit dem Adapter zu kommunizieren. In diesem Beispiel wird MobaXTerm verwendet, aber ein Programm wie Putty od. Ä. funktioniert ebenso.

<img src="resources/MobaXTerm_1.png" width="300">

Schaltet man nun den Router ein, kann man am Computer erneut des Output sehen, den man bereits mit dem Logic-Analyzer bekommen hat.

<img src="resources/MobaXTerm_2.png" width="300">

Drückt man nun "Enter" bekommt man die Information, dass als Shell Busybox verwendet wird. Man kann nun Befehle an den Router schicken.

<img src="resources/MobaXTerm_3.png" width="300">

### Der Exploit

Der Fernzugriff auf den Router kann über die Web-Oberfläche aktiviert werden. Dazu muss man im Netzwerk des Routers sein und das Administrator-Passwort kennen. Um in das Netzwerk des Routers zu kommen, kann man sich einfach mit einem Netzwerkkabel am Router anschließen. Da man ungeschützten Root-Zugriff auf die Konsole des Routers hat, versucht man so das Admin-Passwort ausfindig zu machen. Das vom Router verwendete Linux bietet viele Linux-Funktionen, unter anderem auch den Befehl nvram. Mit diesem Befehl kann man sich die Parameter des vom Router verwendeten NVRAM ansehen. In diesem sind auch alle Zugangsdaten gespeichert. Da es im NVRAM sehr viele Einträge gibt, sucht man mit grep nach Strings die "pass" enthalten, also Einträge, die ein Passwort enthalten. Man sieht nun einige Passwort-Einträge. Beim durchsuchen findet man das http-password, also das gesuchte Administrator-Passwort. Außerdem findet man auch die wla_passphrase, welche das Passwort für das WLAN ist.

<img src="resources/MobaXTerm_4.png" width="300">

Mit Hilfe des WLAN-Passwortes kann man sich nun mit dem WLAN des Routers verbinden, anstatt ein Kabel verwenden zu müssen

<img src="resources/Exploit_1.png" width="300">

Ruft man nun die IP-Adresse des Routers auf (Standardgateway) wird man nach den Zugangsdaten gefragt.

<img src="resources/Exploit_2.png" width="300">

Gibt man nun das in der Konsole gefundene Passwort für den Benutzer admin ein, kommt man zur Konfiguration des Routers.

<img src="resources/Exploit_3.png" width="300">

Unter dem Reiter "Erweitert" findet man einige Informationen des Routers, wie dessen IP-Adresse im "Internet" also dem Netzwerk des TP-Link Routers. Dies ist die Adresse, unter der der Fernzugriff eingerichtet wird.

<img src="resources/Exploit_4.png" width="300">

Unter "Erweiterte Einrichtung" -> "Fernsteuerung" aktiviert man nun den Fernzugriff. Hier auch auch die IP-Adresse und der Port für den Zugriff angezeigt.

<img src="resources/Exploit_5.png" width="300">

Nun verbindet man sich mit dem "Internet", aus welchem der gehackte Router nun erreichbar sein sollte.

<img src="resources/Exploit_6.png" width="300">

Ruft man nun die zuvor abgelesene Adresse und Port auf, erscheint die Anmeldefläche. Dies bedeutet, der Router ist nun aus dem "Internet" erreichbar.

<img src="resources/Exploit_7.png" width="300">

Man meldet erneut mit den Administrator-Zugangsdaten an und hat somit die Challenge gelöst.

<img src="resources/Exploit_8.png" width="300">

</details>
