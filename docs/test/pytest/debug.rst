Debugging
=========

Optionen
--------

+-----------------------+-----------------------------------------------+
| Optionen              | Beschreibung                                  |
+=======================+===============================================+
| :samp:`-s`            | stummschalten                                 |
+-----------------------+-----------------------------------------------+
| :samp:`-v`            | verbos, :samp:`-vv` ist noch ausführlicher    |
+-----------------------+-----------------------------------------------+
| :samp:`--tb`          | Traceback-Stil:                               |
|                       | :samp:`auto|long|short|line|native|no|`       |
|                       |                                               |
|                       | Üblicherweise nutze ich :samp:`--tb=short`    |
|                       | als Standardeinstellung in der                |
|                       | Konfigurationsdatei und die anderen für die   |
|                       | Fehlersuche.                                  |
+-----------------------+-----------------------------------------------+
| :samp:`-x`            | hält beim ersten Fehler an                    |
+-----------------------+-----------------------------------------------+
| :samp:`--lf`          | führt den zuerst fehlgeschlagenen Test aus    |
+-----------------------+-----------------------------------------------+
| :samp:`--ff`          | startet mit dem zuerst fehlgeschlagenen Test  |
|                       | und führt dann alle aus.                      |
+-----------------------+-----------------------------------------------+
| :samp:`--nf`          | führt zuerst neue Testdateien aus, dann den   |
|                       | Rest sortiert nach Änderungsdatum.            |
+-----------------------+-----------------------------------------------+
| :samp:`--sw`          | führt den letzten fehlgeschlagenen Test aus,  |
|                       | stoppt dann beim nächsten Fehler und startet  |
|                       | beim nächsten Mal wieder beim letzten         |
|                       | fehlgeschlagenen Test. Ähnlich wie die        |
|                       | Kombination von :samp:`--lf -x`, aber         |
|                       | effizienter.                                  |
+-----------------------+-----------------------------------------------+
| :samp:`--sw-skip`     | wie oben, aber ein fehlgeschlagener Test      |
|                       | wird übersprungen.                            |
+-----------------------+-----------------------------------------------+
| :samp:`--pdb`         | startet den Python-Debugger im Fehlerfall.    |
|                       |                                               |
|                       | Sehr nützlich zum Debuggen mit :doc:`../tox`. |
+-----------------------+-----------------------------------------------+
