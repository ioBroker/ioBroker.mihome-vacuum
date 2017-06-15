![Logo](admin/mihome-vacuum.png)
ioBroker mihome-vacuum adapter
=================
[![NPM version](http://img.shields.io/npm/v/iobroker.mihome-vacuum.svg)](https://www.npmjs.com/package/iobroker.mihome-vacuum)
[![Downloads](https://img.shields.io/npm/dm/iobroker.mihome-vacuum.svg)](https://www.npmjs.com/package/iobroker.mihome-vacuum)
[![Tests](https://travis-ci.org/ioBroker/ioBroker.mihome-vacuum.svg?branch=master)](https://travis-ci.org/ioBroker/ioBroker.mihome-vacuum)

[![NPM](https://nodei.co/npm/iobroker.mihome-vacuum.png?downloads=true)](https://nodei.co/npm/iobroker.mihome-vacuum/)

This adapter allows you control the Xiaomi vacuum cleaner.

### Install

```
cd /opt/iobroker
npm install iobroker.mihome-vacuum
iobroker add mihome-vacuum
```

## Konfiguration
Derzeit stellt das Ermitteln des Tokens das größte Problem.
Token Vorgehen:

###  Bei Android
Vorbereitung:
Wir brauchen ein Android Handy mit der eingerichteteten Mihome App (Sauger auch schon eingerichtet) und einen Windows PC.

- Nun das [MiToolkit](https://github.com/ultrara1n/MiToolkit/releases) downloaden, endpacken und MiToolkit.exe öffnen.
- USB-Degugging in den Handy-Einstellungen einschalten ([video](https://www.youtube.com/watch?v=aw7D6bNgI1U))
- Das Handy über ein USB-Kabel mit dem PC verbinden.
- in dem MiToolkit Auf Verbindung prüfen klicken und Javainstanz testen, beides sollte Fehlerfrei sein.
- Nun nur noch auf "Token auslesen klicken" und die Meldung auf dem Handy bestätigen.

Auf dem Handy sollte nun die MiHome app geöffnet werden (automatisch) und es wird ein Backup auf den pc gezogen (sollte ein paar Sekunden dauern), das Programm ließt dann aus der Datenbank den Token aus.
Nun nur in dem geöffneten Fenster nach rockrobo.vaccuum suchen und den 32 Stelligen Token kopieren und in dem Konfigurationsfenster eingeben.


###  Bei iOS

Mit Jailbreak:
- Findet man den Token unter /var/mobile/Containers/Data/Application/514106F3-C854-45E9-A45C-119CB4FFC235/Documents/USERID_mihome.sqlite

Ohne Jailbreak:
- Muss man einen unverschlüsselten iTunes Backup machen mit z.B. ([Link](http://www.imactools.com/iphonebackupviewer/)).
- Und dann in den Dateien nach  DB unter RAW, com.xiaomi.home, USERID_mihome.sqlite suchen.


Auch hier wird nach dem 32Stelligen Token gesucht


### 1. Roboter resetten
Um den Token des Roboters zu bekommen, muss man ihn erstmal aus dem Wlan abmelden.
Dies geht entweder über die app unter den Einstellungen des Gerätes
(genauer kann ich es nicht beschreiben weil ich kein Chinesisch kann).
Da steht Remove Device, über diesen Punkt kann man das Gerät Entfernen
Alternativ kann man auch den Resetknopf am Gerät drücken.

### 2. Mit WLAN von Roboter verbinden
Nun verbindet man sich mit seinem Windows (oder anderem Betriebssystem) Rechner mit dem Wlan des Roboters. Der Rechner muss also WLAN haben
Die Kennung des Wlan lautet: rockrobo...

### 3. Packet Sender
Das Programm "Packet Sender" muss installiert sein und gestartet werden

- Bei müssen folgende Werte eingegeben werden
- HEX: 21310020ffffffffffffffffffffffffffffffffffffffffffffffffffffffff (=HALO Nachricht)
- IP Vacuum 192.168.8.1
- Port 54321
- Nachrichtentyp UDP

### 5. Packet senden.
Fenster unten:

a) gesetzte Nachricht
b) darüber die Antwort von dem Roboter

### 6. Antwort auswerten
Die letzten 16 Byte der Antwort sind der gesuchte Token

## Widget
![Widget](widgets/mihome-vacuum/img/previewControl.png)

## Changelog
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
