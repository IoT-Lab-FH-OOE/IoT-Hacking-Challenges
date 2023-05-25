# Challenge 3 - Firmware Analyse

## Thema

In dieser Challenge geht es darum die Firmware eines Routers zu analysieren, um sich Zugriff auf den Router zu verschaffen und eine selbst modifizierte Firmware einzuspielen.

## Benötigte Ressourcen

* Raspberry Pi + Netzteil
* USB-Stick mit Router Firmware & Firmware IDIoT
* WLAN-fähiger Computer

## Die Ausgangslage

Du möchtest dir Zugang zu einem WLAN-Router verschaffen und diesen so manipulieren, dass man das Administrator-Passwort nicht mehr ändern kann.
Du weißt welche Firmware der Router benutzt und dass die Standard Administrator-Zugangsdaten zum Router in der Firmware hartkodiert sind. Mit Hilfe eines speziellen Firmware-Analyse-Tools (FirmwareIDIoT) willst du nun die Firmware analysieren, um die Zugangsdaten zu finden und die Web-Oberfläche so zu manipulieren, dass sich das Passwort nicht mehr ändern lässt.
Nachdem du die Zugangsdaten herausgefunden hast, willst du über die Web-Oberfläche des Routers die manipulierte Firmware einspielen. Durch die neue manipulierte Firmware willst du erreichen, dass du immer Zugriff auf den Router hast, da das Passwort nicht geändert werden kann und selbst bei einem Reset des Routers wieder das Hard-kodierte Passwort verwendet wird.

## Ziele

**Hauptziel:** Manipuliere die Router-Firmware so, dass man das Administrator-Passwort über die Web-Oberfläche nicht mehr ändern kann.<br>
Konkret: Du hast die Challenge gelöst, wenn du dich über die Web-Oberfläche mit dem Administrator Passwort einloggst, auf Passwort ändern gehst, ein neues Passwort festlegst (inkl. Bestätigungsnachricht) und du dich dennoch weiterhin mit dem alten Passwort anmelden kannst.

**Unterziele:**

1. Analysiere die Firmware mit Hilfe des Firmware-IDIoT und beschaffe dir die Administrator (root) Zugangsdaten des Routers
2. Manipuliere die Router-Firmware so, dass man das Administrator-Passwort über die Web-Oberfläche nicht mehr ändern kann. Das Fenster zum Ändern des Passwortes muss allerdings weiterhin erreichbar sein. Beim Ändern des Passwortes soll auch die übliche Meldung erscheinen, dass das Passwort geändert wurde. Im Hintergrund soll allerdings das alte Passwort beibehalten werden.
3. Spiele deine Firmware über den Update-Machanismus des Routers auf den Router und teste, ob deine Manipulation erfolgreich war.

**Nicht-Ziele**:

1. Es ist nicht Ziel während der Challenge Veränderungen direkt über die SD-Karte des Raspberry Pi vorzunehmen.

## Vorbereitung/Allgemeines

Damit die Challenge wiederholbar bleibt und der Router nicht durch eine fehlerhaft manipulierte Firmware dauerhaft beschädigt wird, wird der Router durch einen Raspberry Pi emuliert. Jegliches Hacking muss durch das Einspielen einer Firmware über die Web-Oberfläche geschehen. Solltest du mit einer falsch manipulierten Firmware den Router bricken, kannst du ganz einfach wieder das Image auf die SD-Karte flashen und es nochmal versuchen. Das Firmware-Image (Challenge3_Router.img) findest du auf dem beigelegten USB-Stick.

Auf dem beigelegten USB-Stick findest du auch den Firmware IDIoT, ein im SESAM entwickeltes Firmware Analyse Toolkit. Mit diesem lässt sich Firmware extrahieren, analysieren und wieder zusammenbauen. Du kannst die vorgefertigte Kali VM für Virtualbox verwenden, auf welcher der Firmware IDIoT bereits installiert ist, oder den Source-Code, um ihn selbst auf deinem System zu installieren. Eine kurze Anleitung zur Installation und zur Verwendung des Firmware IDIoT findest du hier: [Firmware IDIoT](Firmware-IDIoT.md)

### Aufbauen der Challenge

1. Flashe das Firmware-Image (Challenge3_Router.img) auf die SD-Karte des Raspberry Pi. Du kannst zum Flashen der SD Karte Tools deiner Wahl verwenden, empfohlen wird das Tool [Raspberry Pi Imager](https://www.raspberrypi.com/software/).
2. Lege die SD-Karte in den Raspberry Pi und verbinde ihn mit dem Stromnetz.
3. Der Raspberry Pi startet automatisch und du kannst mit der Challenge beginnen.

Die Zugangsdaten für das WLAN, dass der Router (Raspberry Pi) ausstrahlt sind:<br>
SSID: Challenge3_Router<br>
Password: Challenge3_Secure<br>

------------------------------------------------------------------------

# Hinweise

Hier findest du Hinweise, falls du beim Lösen der Challenge nicht weiterkommst.

## Hinweis 1

Öffne diesen Hinweis, wenn du nicht weißt, wie du beginnen sollst.

<details>
<summary>Hinweis 1 anzeigen…</summary>
<br>
Verwende den Firmware IDIoT, um das Firmware-Image zu untersuchen und das Filesystem zu extrahieren. Suche nun nach Dateien, die ein Passwort enthalten könnten.
</details>

## Hinweis 2

Öffne diesen Hinweis, wenn du nicht weißt, wie du das Passwort decryptest.

<details>
<summary>Hinweis 2 anzeigen…</summary>
<br>
Verwende ein Hash-Crack Tool wie Hashcat oder John the Ripper, um den Passwort-Hash zu decrypten.
</details>

## Hinweis 3

Öffne diesen Hinweis, wenn du nicht weißt, was du in der Firmware manipulieren sollst.

<details>
<summary>Hinweis 3 anzeigen…</summary>
<br>
Finde die Datei www/luci-static/resources/view/system/password.js und verändere diese so, dass beim Ändern des Passwortes die übliche Meldung erscheint, dass das Passwort geändert wurde, aber im Hintergrund das alte Passwort beibehalten wird.
</details>
