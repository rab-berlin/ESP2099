# Bring deinen Busch Microtronic 2090 ins 21. Jahrhundert!

![Prototyp](/pics/IMG_20260403_171612.jpg)

Wir schließen an die Ein- und Ausgänge des 2090 einen ESP32 an, auf dem Micropython läuft. Dadurch können wir beliebige Python-Programme laufen lassen, die mit dem 2090 kommunizieren. 

Vom einfachen Blinklicht über anpassbaren Zufallszahlengenerator, 2095-Tape-Emulator, Tongenerator, beliebige Sensoren, Internet-Anbindung bis hin zu einem 8x8-Matrix-LED-"Bildschirm" für den Microtronic - über die Weboberfläche lässt sich alles konfigurieren und steuern. 

![Webscreen](/pics/IMG_20260403_173029.jpg)

## Schaltung

Die Schaltung des ESP2090-Studios ist verhältnismäßig einfach. 

Der ESP32 arbeitet mit 3,3 Volt Betriebsspannung, der Microtronic hingegen mit 5 Volt (genauer gesagt: sogar 5,7 Volt - nachmessen!). Daher sind in den Signalwegen jeweils Levelshifter eingebaut. Da wir es mit verhältnismäßig langsamen Signalen im Bereich von Millisekunden zu tun haben, muss man über die Schaltgeschwindigkeit der Levelshifter nicht besonders intensiv nachdenken. Billige Teile aus China funktionieren gut.

Ich habe darauf geachtet, dass die Ein- und Ausgänge des Microtronic elektrisch vom ESP entkoppelt sind, das heißt, sie sollten auch bei gleichzeitigem Anschluss des ESP2090-Studios nach wie vor nutzbar bleiben, falls z.B. weitere Peripherie aus dem Busch-Sortiment verwendet wird.

### Eingänge

Die Eingänge des Microtronic sind an je einen Ausgang eines ODER-Gatters (CD4071) angeschlossen. Je ein Eingang eines ODER-Gatters ist mit einem GPIO des ESP32 verbunden, der andere Eingang des Gatters ist "frei" (kann also wie bisher als Eingang des 2090 genutzt werden). So können weiterhin beliebige Peripherie-Schaltungen an den Microtronic angeschlossen werden - ohne dass es zu elektrischer Beeinflussung durch den ESP32 kommt. Die Eingänge des 4071 sind extrem hochohmig, über Ströme muss man sich also keine Gedanken machen.

### Ausgänge 

Die Ausgänge des Microtronic sind an je zwei Eingänge eines 74HCT244 angeschlossen. Dadurch wird ein Microtronic-Ausgang praktisch "verdoppelt" - ein Ausgang bleibt unabhängig nutzbar, kann also beliebige Peripherie ansteuern. Der andere Ausgang ist mit einem GPIO verbunden, sodass der ESP32 alle Veränderungen an den Ausgängen überwachen kann, ohne eventuell angeschlossene weitere Peripherie elektrisch zu stören. Es ist zu beachten, dass der 74HCT244 maximal 20 mA an seinen Ausgängen liefert - mehr sollte auch der Microtronic im Laufe seines Lebens (45 Jahre) niemals geliefert haben, wenn man seine Ausgänge nicht überlasten wollte. Für die Ansteuerung eines Transistors ist das mehr als ausreichend.
