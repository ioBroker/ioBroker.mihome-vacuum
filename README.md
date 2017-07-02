![Logo](admin/mihome-vacuum.png)
ioBroker mihome-vacuum adapter
=================
[![NPM version](http://img.shields.io/npm/v/iobroker.mihome-vacuum.svg)](https://www.npmjs.com/package/iobroker.mihome-vacuum)
[![Downloads](https://img.shields.io/npm/dm/iobroker.mihome-vacuum.svg)](https://www.npmjs.com/package/iobroker.mihome-vacuum)
[![Tests](https://travis-ci.org/ioBroker/ioBroker.mihome-vacuum.svg?branch=master)](https://travis-ci.org/ioBroker/ioBroker.mihome-vacuum)

[![NPM](https://nodei.co/npm/iobroker.mihome-vacuum.png?downloads=true)](https://nodei.co/npm/iobroker.mihome-vacuum/)

This adapter allows you control the Xiaomi vacuum cleaner.

# Install

```
cd /opt/iobroker
npm install iobroker.mihome-vacuum
iobroker add mihome-vacuum
```
## Inhalt
- [Einrichtung](#konfiguration)
    - [mit Android](#bei-android)
    - [mit iOS](#bei-ios)
    - [Adapter konfigurieren](#adapterkonfiguration)
        - [Zweiter Roboter](#zweiter-roboter)
- [Funtionen](#funktionen)
    - [Eigene Kommandos](#sende-eigene-kommandos)
- [Widget](#widget)
- [Bugs](#bugs)
- [Changelog](#changelog)
## Konfiguration
Derzeit stellt das Ermitteln des Tokens das größte Problem.
Folgende Vorgehensweisen können genutzt werden:

### Bei Android
Vorbereitung:
Benötigt wird ein Android Smartphone mit fertig eingerichteter MiHome App. Der Sauger muss in dieser hinzugefügt und eingerichtet sein.

- Das [MiToolkit](https://github.com/ultrara1n/MiToolkit/releases) herunterladen, entpacken und die MiToolkit.exe starten.
- USB-Degugging in den Smartphone-Einstellungen einschalten ([video](https://www.youtube.com/watch?v=aw7D6bNgI1U))
- Das Smartphone über ein USB-Kabel mit dem PC verbinden.
- Im MiToolkit auf "Verbindung prüfen" klicken und ggf. die Java Installation testen, beide Tests sollten fehlerfrei verlaufen.
- Auf "Token auslesen" klicken und die Meldung auf dem Smartphone bestätigen (KEIN Passwort vergeben!).

Auf dem Smartphone sollte nun die MiHome App geöffnet werden (automatisch) und ein Backup auf den PC gezogen werden (sollte ein paar Sekunden dauern), das Programm liest dann den Token aus der MiHome Datenbank (miio2.db) aus.
Nun nur in dem geöffneten Fenster nach rockrobo.vaccuum suchen und den 32 Stelligen Token kopieren und in dem Konfigurationsfenster eingeben.


### Bei iOS

Mit Jailbreak:
- Findet man den Token unter /var/mobile/Containers/Data/Application/514106F3-C854-45E9-A45C-119CB4FFC235/Documents/USERID_mihome.sqlite

Ohne Jailbreak:
- Muss man einen unverschlüsselten iTunes Backup machen mit z.B. ([Link](http://www.imactools.com/iphonebackupviewer/)).
- Und dann in den Dateien nach  DB unter RAW, com.xiaomi.home, USERID_mihome.sqlite suchen.


Auch hier wird nach dem 32 stelligen Token gesucht

### Adapterkonfiguration
- Bei IP-Adresse muss die IP-Adresse des Roboters eingegeben werden im Format "192.168.178.XX"
- Port des Roboters ist Standardmäßig auf "54321" eingestellt, dies sollte nicht verändert werden
- Eigener Port, sollte nur bei zweiten Roboter geändert werden
- Abfrageintervall Ist die Zeit in ms in der die Statuswerte des Roboters abgerufen werden (sollte nicht <10000 sein)

- Experimental: Über den Haken bei "Sende eigene Komandos" werden Objekte angelegt, über die man eigene Kommandos an den Roboter senden und empfangen kann.

#### Zweiter Roboter
Sollen zwei Roboter über ioBroker gesteuert werden, müssen zwei Instanzen angelegt werden. Dabei muss bei dem zweiten Roboter der eigene Port (Default: 53421) geändert werden, damit beide Roboter unterschiedliche Ports besitzen.

## Funktionen
### Sende eigene Kommandos
HINWEIS: Diese Funktion sollte nur von Experten genutzt werden, da durch falsche Kommandos der sauger zu Schaden kommen könnte

Der Roboter unterscheidet bei den Kommandos in Methoden (method) und Parameter(params) die zur spezifizierung der Methoden dienen.
Under dem Object "mihome-vacuum.X.control.X_send_command" können eigene Kommandos an den Roboter gesendet werden.
Der Objektaufbau muss dabei wiefolgt aussehen: method;[params]

Unter dem Objekt "mihome-vacuum.X.control.X_get_response" wird nach dem Absenden die Antwort vom Roboter eingetragen. Wurden Parameter abgefragt erscheinen sie hier im JSON Format, wurde nur ein Befehl gesendet, antwortet der Roboter nur mit "0".

Folgende Methoden und Parameter werden unterstützt:

| method      | params     | Beschreibung                                                                                           |
|-----------  |-------     |-------------------                                                                                     |
| get_timer   |            |       liefert den eingestellten Timer zurück                                                           |
| set_timer   | [["ZEIT_IN_MS",["30 12 * * 1,2,3,4,5",["start_clean",""]]]] |     Einstellen der Saugzeiten BSp. 12 Uhr 30 an 5 Tagen |
| upd_timer   | ["1481997713308","on/off"] |     Timer aktivieren an/aussehen                                                       |
|             |            |                                                                                                       |
| get_dnd_timer  |      |       Lifert die Zeiten des Do Not Distrube zurück|
| close_dnd_timer  |      |       DND Zeiten löschen|
| set_dnd_timer  |   [22,0,8,0]   |       DND Einstellen h,min,h,min|
|             |            |                                                                                                       |
|app_rc_start|            | Romote Control starten                                                                      |
|app_rc_end|            | Romote Control beenden                                                                      |
|app_rc_move|[{"seqnum":'0-1000',"velocity":WERT1,"omega":WERT2,"duration":WERT3}] | Bewegung. Sequenznummer muss fortlaufend sein, WERT1(Geschw.) = -0.3 - 0.3, WERT2(Drehung) = -3.1 - 3.1, WERT3(Dauer)|

## Widget
Zur Zeit leider noch nicht fertig.
![Widget](widgets/mihome-vacuum/img/previewControl.png)

## Bugs
- gelegentliche Verbindungsabbrüche dies liegt jedoch nicht am Adapter sondern meistens am eigenen Netzwerke
- Widget zur Zeit ohne Funktion

## Changelog
### 0.5.5 (2017-06-30)
* (MeisterTR) add states, fetures, fix communication errors

### 0.3.2 (2017-06-07)
* (MeisterTR) fix no communication after softwareupdate(Vers. 3.3.9)

### 0.3.1 (2017-04-10)
* (MeisterTR) fix setting the fan power
* (bluefox) catch error if port is occupied

### 0.3.0 (2017-04-08)
* (MeisterTR) add more states

### 0.0.2 (2017-04-02)
* (steinwedel) implement better decoding of packets

### 0.0.1 (2017-01-16)
* (bluefox) initial commit
