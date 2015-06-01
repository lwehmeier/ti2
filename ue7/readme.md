# Technische Informatik 2 - Übung 7

## Programm für heute:

* Vorstellen Aufgabe 4
* Abgabe praktische Aufgabe 3

## Orga

* theoretische Aufgabe 3 zu naechster Woche
* praktische Aufgabe 4 ab 16.06

## Kurze Einführung in Task4

* Scheduling
	* kooperativ
	* preemptiv
* 3 Tasks
	* Counter 1: zählt hoch
	* Counter 2: zählt hoch
	* rotCursor: lässt Cursor hin- und herspringen

* **Teil A**
	* Kooperatives Multitasking implementieren
	* Wechseln zwischen "Applications" mittely Thread::yield() bzw. Thread::exit()
	* Implementieren Kontextwechsel
	* Implementieren von Kill und Exit für Tasks
	* Empfohlenes Vorgehen:
		* Scheduler::start
		* Scheduler::insert einfügen von Task in die Queue
		* Thread::kickoff(Thread*) startet Thread::action, statische Methode damit vom Scheduler initial aufrufbar
		* Thread::yield lässt Scheduler nächsten Task dispatchen
	* Interessant: Dispatcher::dispatch, ändern des Kontexts und Ausführung fortsetzen

* **Teil B**
	* preemptives Multitasking
	* Taskwechsel in Timerinterrupt (PIT-programmable interrupt timer)
	* implementieren von "Watch" für die Interrupts
	* Schutz der Zugriffe auf die Queue notwendig, u.a. da yield weiterhin möglich
	* Scheduler::preempt und Watch::trigger
	* Empfohlenes Vorgehen:
		* Implementieren von Watch, Testen z.b. durch periodische Ausgabe oder Breakpoint
		* preempt implementieren
		* Schutz der Queue durch Lock
			* InterruptLock lock/unlock locken Interrupts bzw. schalten diese wieder frei, doppelten Aufruf vermeiden!!
			* ScopedLock(InterruptLock) erzeugen, lockt Interrupts bis zum Aufruf des Destruktors, also verlassen des Scopes. Gut zum Schutz von ganzen Funktionen. Schachtelung möglich
			

## Hilfestellung zu Aufgabe 4

Informationen zu finden sich in der Dokumentation unter `doc/html/task4.html`
Grundlegend:
* PIT quasi fertig
* eher etwas knobeln mit den Locks
* In Interrupt/Exception Interrupts standardmäßig aus
* Bei Aufruf durch yield() standardmäßig an
* ScopedLock
	* Nur erster Lock hat Effekt
	* Weitere Locks harmlos im Gegensatz zu InterruptLock::lock
	* Unlock erfolgt nur durch Destruktor der lockenden Instanz

### Ressources

Dieses mal keine weiteren Ressourcen, hier sind kaum hardwarespezifische Funktionen nötig, nur etwas Denken für die Locks und Scheduler::preempt
[Intel manuals](http://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)


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