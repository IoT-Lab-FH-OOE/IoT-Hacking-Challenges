# Der Firmware IDIoT

## Installation

### Vrtualbox VM

In der vorbereiteten VM läuft Kali 2023.1 und alle Abhängigkeiten des Firmware IDIoT sind bereits installiert.

User: `kali`<br>
Passwort: `kali`

Der Firmware IDIoT ist zu finden unter `/home/kali/firmware-idiot`

Das zu analysierende Firmware Image (Challenge3_Router.img) ist bereits in der VM (firmware-idiot/images).

### Manuelle Installation

Um den Firmware IDIoT zu verwenden, müssen zuvor einige Bibliotheken installiert werden.

Der Firmware IDIoT benötigt folgende Bibliotheken (mindestens angegebene Version):

- Python 3.5.2
- IPython 6.2.1
- binwalk v2.1.2-ce7f1f9
- python-magic 0.4.15
- dill 0.2.7.1
- peewee 3.0.17
- hexdump 3.3
- matplotlib 3.0.3

Dazu befindet sich im Root-Ordner des Firmware IDIoT die Datei requirements.txt. Mit dem Befehl `sudo pip3 install -r requirements.txt` werden alle benötigten Abhängigkeiten außer Binwalk installiert.

#### Installation Binwalk

Da es bei der Installation von Binwalk des öfteren zu Problemen kommt, ist folgende Installationsvorgehensweise empfohlen:
```
git clone https://github.com/ReFirmLabs/binwalk.git
cd binwalk
sudo python3 setup.py install
sudo ./deps.sh
```

## Verwendung des Firmware IDIoT

### Firmware IDIoT starten
Der Firmware IDIoT muss im Verzeichnis `firmware-idiot/idiot` gestartet werden!
```
sudo ./run_idiot.sh
```
### Firmware extrahieren
Als erstes gibt man das Firmware image an. Der Firmware IDIoT versucht nun den Firmware Header zu identifizieren, dies dauert einen kurzen Moment.
```
firmware = Firmware('path/to/image')
```
Anschließend kann man mit dem do_extract Befehl die firmware extrahieren lassen. Hierbei zerlegt der Firmware IDIoT die Firmware in seine Sektionen. Das Extrahieren dauert einen Moment und es gibt währenddessen kein Feedback zum Fortschritt. Einfach kurz abwarten, bis die Input-Zeile wieder auftaucht.
```
firmware.do_extract()
```
### Filesystem extrahieren
Um das Filesystem der Firmware zu analysieren, muss dieses zuvor extrahiert werden. Dazu wählt man die Filesystem-Sektion aus den zuvor extrahierten Sektionen aus.
```
filesystem = firmware.children[filesystem-index]
filesystem.do_extract()
```
Das extrahierte Filesystem befindet sich im Ordner /root/.idiot/.idiot_session_\<date>

### Firmware verändern
Man kann nun Veränderungen am Filesystem der Firmware vornehmen und anschließend die Firmware wieder zusammenbauen.

### Firmware builden
Um die Firmware wieder zusammenzubauen muss zuerst die Filesystem-Sektion wieder zusammengebaut werden und anschließend alle Sektionen wieder zu einer ganzen Firwmare.
```
filesystem.do_build()
firmware.do_build('/tmp/')
```
Die neue Firmware ist nun im Ordner ```/tmp``` zu finden und hat die Dateiendung .img_patched_\<id>
