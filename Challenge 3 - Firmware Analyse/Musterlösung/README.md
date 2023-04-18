$$\Large{\color{red}Spoiler \space ahead!}$$

> __Warning__
Nachfolgend ist die Musterlösung der Challenge 3.
Nicht weiterlesen, wenn du diese Challenge noch absolvieren möchtest! Hinweise zum Lösen der Challenge findest du am Ende der Angabe.

<details>
<summary>Musterlösung anzeigen...</summary>
<br>

### Root Passwort finden

Mit Hilfe des Firmware IDIoT wird die Firmware des Routers, wie in der [Firmware IDIoT Anleitung](/Challenge%203%20-%20Firmware%20Analyse/Firmware-IDIoT.md) beschrieben, entpackt.

Im entpackten Filesystem unter ``/root/.idiot/.idiot_session_<date>`` navigiert man nun zur Datei ``/etc/shadow``. In dieser findet sich der String ``root:$1$yC6Jtuds$djIsU.aDJtlFpCQonPlK00:19360:0:99999:7:::``, was der Passwort-Hash des root users ist.<br>

### Root Passwort decrypten

Der Paswort Hash lässt sich ganz einfach mittels John the Ripper decrypten: ``john ./shadow`` und erhält das Passwort: ``gangsta``

### Weboberfläche bearbeiten

Mit dem dercypteten Passwort meldet man sich nun an der Weboberfläche des Routers an und navigiert zum Reiter Administration -> Passwort. Schaut man sich nun den Seitenquelltext an, findet man heraus, dass das Java Script password.js aufgerufen wird.<br>

Man sucht nun im extrahierten Filesystem nach dieser Datei und bearbeitet sie so, dass das Passwort nicht geändeert wird, wenn man es über die Weboberfläche ändern möchte. Dazu 

</details>
