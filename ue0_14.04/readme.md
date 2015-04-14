# Technische Informatik 2 - Einführungsübung

## Programm für heute:

* virtuelle Maschine / Buildumgebung + Toolchain aufsetzen
* Kurze Einführung in OOStuBs / Erste Aufgabe
  * Hilfestellung für erste Aufgabe

## Zu meiner Person

Leon Wehmeier  
leon.wehmeier@st.ovgu.de  
Ing-Inf  

## Fragen?

* orga, etc?

## VM aufsetzen

* Vorraussetzung ist Virtualbox
* Image der VM gibt es zum Download hier:  
  * http://www-e.uni-magdeburg.de/zug/TI-virtual_system.zip

## alternativ: Toolchain/Emulator aufsetzen

**Achtung: Eine nicht funktionierende Toolchain wird von uns nicht als Entschuldigung für Verspätungen anerkannt.**


Ihr braucht im Wesentlichen (getestet auf Debian jessie):

* `build-essential`
* `gcc` (`gcc-multilib` auf amd64)
* `g++` (`g++ multilib` auf amd64)
* `qemu`
* `qemu-system`
* `libc6-dev` (`libc6-dev-i386` auf amd64)
* `doxygen` (für Dokumentation)


## Kurze Einführung in OOStuBs

Es gibt eine Hilfe, die auf Doxygen basiert, dort sind alle Klassen und technischen Hintergründe zu den Aufgaben noch einmal genau erklärt. 
Ihr könnt die Hilfe mit
```sh
make doc
```
bauen, danach findet ihr im Ordner `doc` die Datei `html/index.html` in der die Aufgaben und die Klassen erklärt sind.

**Für jetzt** braucht ihr im Wesentlichen folgende Klassen:

* `Stringbuffer`, hier sollt ihr einen Buffer für Strings implementieren
* `O_Stream`, hier sollt ihr Ausgabefunktionen für alle grundlegenden Datentypen implementieren (z.B. `int`/`uint` in verschiedenen Größen, `void*` oder auch `char*`)
	* zusätzlich soll euer O_Stream auch Zahlen zu verschiedenen Basen (bin, oct, dec, hex) ausgeben können
* `CGA_Screen`, hier sollt ihr eine Low-Level API implementieren, die sich darum kümmert, dass die Farben und das Blinken korrekt gesetzt sind.
* `CGA_Stream`, hier sollt ihr die eigentliche Ausgabe auf den Framebuffer machen.
*  Am Rande: `Log`, hilfreich für Debugausgaben auf der Konsole
	* Ihr müsst vorher `Stringbuffer` und `O_Stream` implementieren, damit ihr `Log` nutzen könnt, das sollte aber nicht das Problem sein.

## Hilfestellung zu Aufgabe 1

Informationen zu CGA finden sich in der Dokumentation unter `doc/html/task1_cgaInfo.html`
Grundlegend:

* **Text**
* Grafikkarte ist im Textmodus (80x25 Zeichen)
* Dabei belegt jedes Zeichen 2 Byte im VRAM
* VRAM ist an 0xB8000 im RAM eingeblendet
* Im ersten Byte steht der ASCII Code für das Zeichen an der entsprechenden Stelle
* Zweites Byte speichert Attribute pro Zeichen
	* Bit 0-3 Vordergrundfarbe
	* Bit 4-6 Hintergrundfarbe
	* Bit 7 Blinken
* BSP:
	* Schreiben ein grünes **A** an die Stelle (0, 0)
    * ``` *((char*)0xB8000) = 'A';```
    * ``` *((char*)0xB8001) = 2; //grün```
   
* **Cursor**
* Position ist Integer der folgenden Form
	* `y_position*80+x_position`
* Steuerregister
	* 14 Cursor(high)
	* 15 Cursor(low)
* Zugriff via Port
	* 3d4 Indexregister
	* 3d5 Datenregister
	* von uns bereits gekapselt -> kein asm nötig ;)
* BSP:
	* Cursor an Stelle 75
	* `IO_Port ioIndex(3d4)`
	* `IO_Port ioData(3d5)`
	* `ioIndex.outb(14)`
	* `ioData.outb(0)`
	* `ioIndex.outb(15)`
	* `ioData.outb(75)`

### OOStuBs bauen

```sh
make all
```

### OOStuBs ausführen in QEMU
```sh
make run
```