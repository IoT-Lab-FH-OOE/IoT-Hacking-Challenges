$$\Large{\color{red}Spoiler \space ahead!}$$

> __Warning__
Nachfolgend ist die gesamte Dokumentation der Challenge 3. Dies inkludiert auch Konfigurationen bzw. Zugangsdaten, die im Laufe der Challenge ausfindig zu machen sind.
Nicht weiterlesen, wenn du diese Challenge noch absolvieren möchtest!

<details>
<summary>Dokumentation der Implementierung anzeigen...</summary>
<br>

### Konfiguration des Raspberry Pi

**Betriebssystem:** OpenWRT-22.03.3-bcm27xx-bcm2711-rpi-4-squashfs

**Zugangsdaten:**<br>
User: root<br>
Password: gangsta<br>
<br>
SSID: Challenge3_Router<br>
Password: Challenge3_Secure

### Installation

https://circuitdigest.com/microcontroller-projects/diy-router-using-raspberry-pi

- Download Software: https://firmware-selector.openwrt.org/?version=22.03.3&target=bcm27xx%2Fbcm2711&id=rpi-4
- Image auf Raspberry Pi flashen
- Raspberry Pi über LAN mit Computer verbinden und einschalten
- Über SSH zu root@192.168.1.1 verbinden (Passwort leer lassen)
- Passwort auf "gangsta" setzen mit passwd
- editiere /etc/config/network
- folgende Zeilen unter interface lan hinzufügen:
```
option dns '192.168.1.1'
option gateway '192.168.1.1'
```
- editiere /etc/config/wireless
```
Ändere die disabled option  von '1' auf '0'
Ändere die encryption von 'none' auf 'psk2'
Ändere die SSID auf 'Challenge3_Router'
```
Füge ``option key 'Challenge3_Secure'`` hinzu
- Raspberry Pi neu starten

</details>
