None
====

Zusätzlich zu den Standardtypen wie :doc:`strings/index` und :doc:`numbers`
verfügt Python über einen speziellen Datentyp, der ein einziges spezielles
Datenobjekt namens ``None`` definiert. Wie der Name schon sagt, wird ``None``
verwendet, um einen leeren Wert darzustellen. Er taucht in verschiedenen Formen
in Python auf. Eine Prozedur in Python ist beispielsweise nur eine Funktion, die
nicht explizit einen Wert zurückgibt, was bedeutet, dass sie standardmäßig
``None`` zurückgibt.

``None`` ist in der alltäglichen Python-Programmierung oft als Platzhalter
nützlich, um eine Datenstruktur zu kennzeichnen, an der irgendwann sinnvolle
Daten gefunden werden können, auch wenn diese Daten noch nicht berechnet wurden.
Das Vorhandensein von ``None`` lässt sich leicht überprüfen, da es im gesamten
Python-System nur eine Instanz von ``None`` gibt (alle Verweise auf ``None``
verweisen auf dasselbe Objekt), und ``None`` ist nur mit sich selbst identisch.
