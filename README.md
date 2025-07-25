You will find the English ReadMe at the end of the document.

# BLE Log Service
Erweiterung für den Bluetooth-Abruf und die Änderung des Datenloggers vom Calliope mini 3.  

# Den Service aktivieren
```sig
blelog.startBLELogService(passphrase?: string) : void
```
Startet den BLE Log Service (mit optionalem Passwort). Dies sollte in den Start-Blöcken eingefügt werden.
Wenn ein Passwort leer ist, wird kein Passwort benötigt, damit andere Geräte Daten abrufen können. Wenn ein Passwort angegeben wird, müssen Geräte das Passwort angeben, um die Daten über Bluetooth abzurufen. (Das Passwort beeinflusst nicht den Zugriff auf die Datenlog-Datei auf dem Calliope mini 3). Wenn ein Passwort länger als 20 Zeichen ist, werden nur die ersten 20 Zeichen verwendet.

# Beispiel
```block
blelog.startBLELogService()
```

# Daten abrufen
Derzeit können Daten von einem browser-basierten Client abgerufen werden (derzeit nur Chrome und Edge): [https://calliope-edu.github.io/webblelog/](https://calliope-edu.github.io/webblelog/).
Eine JavaScript-Bibliothek ist verfügbar, um Benutzeroberflächen usw. zu entwickeln: [https://github.com/calliope-edu/webblelog](https://github.com/calliope-edu/webblelog).
Im Original ist diese hier zu finden: https://github.com/bsiever/microbit-webblelog

# Bluetooth Service Übersicht 
## Service UUID
accb4ce4-8a4b-11ed-a1eb-0242ac120002

## Charakteristiken
| UUID | Name |  Eigenschaften |
|:----:|------|-------------|
|accb4f64-8a4b-11ed-a1eb-0242ac120002 | Sicherheit  | Lesen, Benachrichtigen|
|accb50a4-8a4b-11ed-a1eb-0242ac120002 | Passwort |Schreiben |
|accb520c-8a4b-11ed-a1eb-0242ac120002 | Datenlänge       | Lesen, Benachrichtigen |
|accb53ba-8a4b-11ed-a1eb-0242ac120002 | Daten       | Benachrichtigen |
|accb552c-8a4b-11ed-a1eb-0242ac120002 | Datenanfrage       |Schreiben|
|accb5946-8a4b-11ed-a1eb-0242ac120002 | Löschen        | Schreiben |
|accb5be4-8a4b-11ed-a1eb-0242ac120002 | Nutzung        | Lesen, Benachrichtigen |
|accb5dd8-8a4b-11ed-a1eb-0242ac120002 | Zeit        | Lesen |

## Sicherheit
Lesen/Benachrichtigen zeigt `true` (0xFF) an, wenn die Interaktionen autorisiert sind, und `false` (0x00) andernfalls. (Sendet automatisch den Wert, wenn Benachrichtigungen aktiviert sind)
Falls `false`, ist ein korrektes Passwort erforderlich, um den Zugriff auf andere Daten zu ermöglichen. 

## Passwort
Charakteristikum zum Schreiben des Passworts (falls erforderlich). Wenn ein Passwort erforderlich ist und das richtige angegeben wird, sind andere Charakteristiken verfügbar. Der Zugriff wird bis zur Trennung gewährt.
Das erforderliche Passwort kann im `blelog.startBLELogService()` Block angegeben werden. Standardmäßig (leeres Passwort) wird kein Passwort erforderlich sein. Passwörter sind maximal 20 Zeichen lang.

## Datenlänge
Die Länge der Daten in Bytes (`uint32_t`).

## Daten
Benachrichtigungen sind 0-20 Bytes lang. Eine leere (Länge 0) zeigt das Ende der übertragenen Daten an. Falls nicht leer, sind die ersten 4 Bytes der Index in die Nachricht und die verbleibenden Bytes sind die Daten ab diesem Index (bis zu 16 Bytes Daten).
## Datenanfrage
Zwei `uint32_t`s:  
 
* Ein `uint32_t`, das den Index in das Log angibt, das abgerufen werden soll.  
* Ein `uint32_t`, das die Länge (in Bytes) angibt, die abgerufen werden soll.  
Daten werden über die Daten-Charakteristik-Benachrichtigungen gesendet (accb**53ba**-8a4b-11ed-a1eb-0242ac120002)

## Löschen
Schreibe das vollständige Wort "ERASE", um die Log-Datei zu löschen. (Löscht nur, wenn autorisiert)

## Nutzung
10-mal der Prozentsatz des Logs, der derzeit verwendet wird (`uint16_t` von [0-1000], wobei 1000=100.0). 

## Zeit
Die aktuelle Calliope mini 3 Uhr (Millisekunden) als `uint64_t`.

# Danksagungen 
Icon basiert auf [Font icon 0xF147](https://www.iconfinder.com/search?q=f0a7) SVG.
<script src="https://makecode.com/gh-pages-embed.js"></script>
<script>makeCodeRender("{{ site.makecode.home_url }}", "{{ site.github.owner_name }}/{{ site.github.repository_name }}");</script>


# BLE Log Service

Extension for Bluetooth retrieval and modification of the Calliope mini 3 data logger's log.  

# Enabling the service

```sig
blelog.startBLELogService(passphrase?: string) : void
```

Start the BLE Log service (with optional passphrase).  This should be included in the start blocks.

If a passphrase is empty, no passphrase is required for other devices to retrieve data.  If a passphrase is provided, devices must provide the passphrase to retrieve the data over Bluetooth. (The passphrase does not impact access to the data log file on the Calliope mini 3).  If a passphrase is longer than 20 characters, only the first 20 characters are used.

# Example

```block
blelog.startBLELogService()
```

# Retrieving Data

Currently data can be retrieved by a browser based client (currently Chrome and Edge only):  [https://bsiever.github.io/microbit-webblelog/](https://bsiever.github.io/microbit-webblelog/).

A JavaScript library is available to develop User Interfaces, etc.: [https://github.com/bsiever/microbit-webblelog](https://github.com/bsiever/microbit-webblelog)
The original version can be found here: https://github.com/bsiever/microbit-webblelog

# Bluetooth Service Overview 


## Service UUID

accb4ce4-8a4b-11ed-a1eb-0242ac120002
## Characteristics

| UUID | Name |  Properties |
|:----:|------|-------------|
|accb4f64-8a4b-11ed-a1eb-0242ac120002 | Security  | Read, Notify|
|accb50a4-8a4b-11ed-a1eb-0242ac120002 | Passphrase |Write |
|accb520c-8a4b-11ed-a1eb-0242ac120002 | Data Length       | Read, Notify |
|accb53ba-8a4b-11ed-a1eb-0242ac120002 | Data       | Notify |
|accb552c-8a4b-11ed-a1eb-0242ac120002 | Data Request       |Write|
|accb5946-8a4b-11ed-a1eb-0242ac120002 | Erase        | Write |
|accb5be4-8a4b-11ed-a1eb-0242ac120002 | Usage        | Read, Notify |
|accb5dd8-8a4b-11ed-a1eb-0242ac120002 | Time        | Read |

## Security

Read/notify will indicate a `true` (0xFF) if the interactions are authorized and `false` (0x00) otherwise. (Automatically sends value when notifies are enabled)

If `false`, a correct passphrase is required to enable access to other data. 

## Passphrase

Characteristic to write passphrase (if needed).  If a passphrase is required and the correct one is provided, other characteristics will be available.  Access will be granted until disconnection.

The passphrase that is required can be provided in the `blelog.startBLELogService()` block.  By default (empty passphrase) a passphrase will not be required.  Passphrases are no more than 20 characters.

## Data Length

The length of the data in bytes (`uint32_t`).

## Data

Notifiations will be 0-20 bytes.  An empty (length 0) indicates the end of transmitted data.  If non-empty, the first 4 bytes are the index into the message and the remaining bytes are the data starting at that index (up to 16 bytes of data).

## Data Request

Two `uint32_t`s:  
 
* A `uint32_t` indicating the index into the log to retrieve.  
* A `uint32_t` indicating the length (in bytes) to retrieve.  

Data is sent via the Data characteristic notifications (accb**53ba**-8a4b-11ed-a1eb-0242ac120002) 

## Erase

Write the full word "ERASE" to erase the log file. (It only erases if authorized)

## Usage

10 times the percentage of log currently in use (`uint16_t` from [0-1000], where 1000=100.0). 

## Time

The current Calliope mini 3 clock (milliseconds) as a `uint64_t`.

# Acknowledgements 

Icon based on [Font icon 0xF147](https://www.iconfinder.com/search?q=f0a7) SVG.

<script src="https://makecode.com/gh-pages-embed.js"></script>
<script>makeCodeRender("{{ site.makecode.home_url }}", "{{ site.github.owner_name }}/{{ site.github.repository_name }}");</script>
