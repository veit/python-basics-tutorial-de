Eingebaute Module für Zeichenketten
===================================

Die Python-Standardbibliothek enthält eine Reihe eingebauter Module, mit denen
ihr Zeichenketten managen könnt:

.. _string-modules:

+-----------------------+-------------------------------------------------------------------------------+
| Modul                 | Beschreibung                                                                  |
+=======================+===============================================================================+
| :py:mod:`string`      | vergleicht mit Variablen wie :py:data:`string.digits` oder                    |
|                       | :py:data:`string.whitespace`                                                  |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`re`          | sucht und ersetzt Text mit regulären Ausdrücken                               |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`struct`      | konvertiert zwischen Python-Werten und C-Strukturen, die als                  |
|                       | Python-Bytes-Objekte dargestellt werden.                                      |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`difflib`     | hilft beim Berechnen von Deltas, beim Auffinden von Unterschieden zwischen    |
|                       | Zeichenketten oder Sequenzen und beim Erstellen von Patches und Diff-Dateien  |
+-----------------------+-------------------------------------------------------------------------------+
| :py:mod:`textwrap`    | umbricht und füllt Text, formatiert Text mit Zeilenumbrüchen oder Leerzeichen |
+-----------------------+-------------------------------------------------------------------------------+

.. _end-string-modules:

.. seealso::
   * :doc:`Manipulation von Zeichenketten mit pandas
     <Python4DataScience:workspace/pandas/string-manipulation>`
   * `humanize <https://humanize.readthedocs.io/en/stable/>`_

.. toctree::
   :titlesonly:
   :hidden:

   string
   re
