# Technische Informatik 2 - Übung 3

## Programm für heute:

* Vorstellen Aufgabe 3
* Abgabe praktische Aufgabe 2

## Orga

* theoretische Aufgabe 2 kommt am 11.05
* praktische Aufgabe 3 zu KW27

## Kurze Einführung in Task3

- Debugging
	- **gdb** statt Eclipseintegration
	- `make debug`
- Stackanalyse
	- Stackframe
- **Teil A**
	- Wo tritt der "Fehler" auf?
	- Was für ein Fehler trat auf?
	- Vorgehen
	- Demonstration mit gdb
- **Teil B**
	- Fibonacci Berechnung
	- printStack() an relevanten Punkten
	- Erklären (jedes) Stackdumps
	- Parameterübergabe
	- Arrayaufbau 

## Hilfestellung zu Aufgabe 2

Informationen zu finden sich in der Dokumentation unter `doc/html/task3.html`
Grundlegend:
- Exceptions
	- Double/Triple Fauult
	- Invalid Opcode
	- Stackframe
	- `error code | X | eip | cs | eflags`
- wichtige Register
	- ebp - (Extended) base pointer
	- esp - (Extended) stack pointer
	- eip - (Extended) instruction pointer
	- general purpose - eax, ebx, ecx, edx
		- untere Bytes einzeln ansprechbar als
			- ax (short)
			- ah (byte, high)
			- al (byte, low)
- Segmente
	- cs - Codesegment
	- ds - Datensegment
	- ss - Stacksegment
	- weitere general purpose
- BSP function call
```asm
	push %eax     ; zweiter Parameter fuer f1
    push %ebx     ; erster Parameter  fuer f1
    call f1
    add $8, %esp  ; Parameter vom Stack entfernen
```


|Interrupt  |	Bezeichnung 				|	Typ 	|	Fehlercode  |
|-----------|-------------------------------|-----------|---------------|
|			| Divide Error 					| Fault		| ×				|
|1			| Debug/Reserved for Intel 		| Fault/Trap| ×				|
|2			| Nonmaskable Interrupt 		| Interrupt | ×				|
|3			| Breakpoint 					| Trap		| ×				|
|4			| Overflow 						| Trap		| ×				|
|5			| Bound Range Exceede			| Fault 	| ×				|
|6			| Invalid Opcode 				| Fault 	| ×				|
|7			| Device Not Available 			| Fault 	| ×				|
|8			| Double Fault 					| Abort 	| √				|
|9			| Reserved (ehemals Coprozessor)| Fault 	| ×				|
|10			| Invalid TSS 					| Fault 	| √				|
|11			| Segment Not Present 			| Fault 	| √				|
|12			| Stack-Segment Fault 			| Fault 	| √				|
|13			| General Protection 			| Fault 	| √				|
|14			| Page Fault 					| Fault 	| √				|
|15			| Reserved for Intel 			| ¿ 		| ×				|
|16			| x87 FPU Floating-Point Error 	| Fault 	| √				|
|17			| Alignment Check 				| Fault 	| ×				|
|18			| SIMD Floating-Point Exception | Fault 	| ×				|
|20			| Virtualization Exception 		| Fault 	| x 			|


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