Bedingte Anweisungen
====================

Der Codeblock nach der ersten wahren Bedingung einer ``if``- oder
``elif``-Anweisung wird ausgeführt. Wenn keine der Bedingungen wahr ist, wird
der Codeblock nach dem ``else`` ausgeführt:

.. code-block:: pycon
   :linenos:

   >>> x = 1
   >>> if x < 1:
   ...     x = 2
   ...     y = 3
   ... elif x > 1:
   ...     x = 4
   ...     y = 5
   ... else:
   ...     x = 6
   ...     y = 7
   ...
   >>> x, y
   (6, 7)

Python verwendet Einrückungen, um Blöcke abzugrenzen. Es sind keine expliziten
Begrenzungszeichen wie Klammern oder geschweifte Klammern erforderlich. Jeder
Block besteht aus einer oder mehreren Anweisungen, die durch Zeilenumbrüche
getrennt sind. Alle diese Anweisungen müssen auf der gleichen Einrückungsebene
stehen.

Zeile 5
    Die ``elif``-Anweisung sieht aus wie die ``if``-Anweisung und funktioniert
    auch so, allerdings mit zwei wesentlichen Unterschieden:

    * ``elif`` ist nur nach einer ``if``-Anweisung oder einer anderen
      ``elif``-Anweisung zulässig
    * Ihr könnt so viele ``elif``-Anweisungen verwenden, wie ihr benötigt

Zeile 8
    Die optionale ``else``-Klausel bezeichnet einen Codeblock, der nur dann
    ausgeführt wird, wenn die anderen bedingten Blöcke, ``if`` und ``elif``,
    alle unzutreffend sind.

Darüber hinaus können die Bedingungen logisch miteinander verknüpft werden, zum
Beispiel:

.. code-block:: pycon

   >>> if (
   ...     len(filename) > 11
   ...     and filename.startswith("pylock.")
   ...     and filename.endswith(".toml")
   ... ):
   ...     name = filename.removeprefix("pylock.").removesuffix(".toml")
   ...

Checks
------

* Wenn ihr überprüfen möchtet, ob ein Dateiname mit ``pylock.`` beginnt und auf   ``.toml`` endet, welche Methode würdet ihr verwenden, um den Namen der
  Umgebung abzurufen, die darin enthalten sein könnte, zum Beispiel ``prod`` in
  ``pylock.prod.toml``?
