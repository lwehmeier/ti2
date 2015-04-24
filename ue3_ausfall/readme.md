# Technische Informatik 2 - Übung 3

## Programm für heute:

* kurzes Vorstellen Aufgabe 2, mehr in 2 Wochen
* Hilfe zu Aufgabe 1

## Orga

* theoretische Aufgabe 1 kommt am 27.05
* praktiche Aufgabe 2 zum 15.05

## Kurze Einführung in Task2
- Interrupts
	- PIC
	- Keyboard
	- unterbrechen von Task2

## Hilfestellung zu Aufgabe 2

Informationen zu finden sich in der Dokumentation unter `doc/html/task2.html`
Grundlegend:

* **CPU**
* **PIC**
	* **PIC M/S-System und CPU-Anbindung anzeichnen**
	* programmable interrupt controller
	* ursprünglich ein PIC mit 8 Interrupts
	* dann 2 PICs als Chain, also 15 Interrupts
	* Master/Slave
		* Slave an Kanal 2
	* IO Ports an **20/21** und **A0/A1**
	* Maskieren
		* PIC hat 8-Bit Mask Register um Interrupts zu maskieren
		* Port 21/A1
	* funktionen
		* ack: Interrupt bestätigen
			* immer PIC1, wenn Interrupt von PIC2 auch an PIC2
		* allow: zugehöriges Bit in Maske löschen
			* `mask &= 0xff ^ (1 << BITNUM);`
			* `mask &=~ (1 << BITNUM); //löschen`
			* `mask |= (1 << BITNUM); //setzen`
			* **an PIC2-Leitung denken**
		* forbid
			* Bit in maske setzen
	* `doc/html/task2_pic.html`
	* `wiki.osdev.org/8259_PIC`
	* `lowlevel.eu/wiki/Programmable_Interrupt_Controller`
```c
enum Interrupts{
          timer         = 32,  
          keyboard      = 33,  
          pic2          = 34,  
          serial2       = 35,  
          serial1       = 36,  
          soundcard     = 37,  
          floppy        = 38,  
          parallelport  = 39,  
          rtc           = 40,  
          misc          = 41,  
          ata4          = 42, 
          ata3          = 43, 
          secondps2     = 44, 
          fpu           = 45, 
          ata1          = 46, 
          ata2          = 47  
        };
```

* **InterruptStorage**
* **InterruptManager**
* **InterruptHandler**
* **Panic**
	* Pointer auf Interrupthandler für jeden möglichen Interrupt in IStorage
	* Initial alle handler auf Panic
* **Keyboard**
	* Tastaturinterrupt behandeln
	* "Tastaturtreiber"
	* Reboot mit Ctrl-Alt-Q
* **Artefakte**
	* Wenn alles funktioniert werdet ihr einen durchlaufenden Balken sehen
	* Bei Tastendrücken werden die Zeichen unten angezeigt
	* Es werden Artefakte auftreten
		* Diese sind keine Fehler sondern Teil der Aufgabe
		* Woher kommen sie?
		* Was kann man dagegen unternehmen?


### OOStuBs bauen

```sh
make all
```
### OOStuBs Doku bauen

```sh
make doc
```

### OOStuBs ausführen in QEMU
```sh
make run
```