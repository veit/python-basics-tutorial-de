Code-Blöcke
===========

Code-Blöcke lassen sich mit der Direktive :rst:dir:`code-block` sehr einfach
darstellen. Zusammen mit `Pygments <http://pygments.org/>`_ hebt Sphinx dann die
jeweilige Syntax automatisch hervor. Die passende Sprache für einen Code-Block
könnt ihr angeben mit

.. rst:directive:: .. code-block:: LANGUAGE

   Ihr könnt dies :abbr:`z.B. (zum Beispiel)` so verwenden:

   .. code-block:: rest

      .. code-block:: python

         import this

   .. rubric:: Optionen

   .. rst:directive:option:: linenos

      Für :rst:dir:`code-block` kann mit der ``linenos``-Option auch angegeben
      werden, dass der Code-Block mit Zeilennummern angezeigt werden soll:

      .. code-block:: rest

         .. code-block:: python
            :linenos:

            import this

      .. code-block:: python
         :linenos:

         import this

   .. rst:directive:option:: lineno-start

      Die erste Zeilennummer kann mit der ``lineno-start``-Option ausgewählt
      werden; ``linenos`` wird dann automatisch aktiviert:

      .. code-block:: rest

         .. code-block:: python
            :lineno-start: 10

            import antigravity

      .. code-block:: python
          :lineno-start: 10

          import antigravity

   .. rst:directive:option:: emphasize-lines

      ``emphasize-lines`` erlaubt euch, einzelne Zeilen hervorzuheben.

.. rst:directive:: .. literalinclude:: FILENAME

   erlaubt euch, externe Dateien einzubinden.

   .. rubric:: Optionen

   .. rst:directive:option:: emphasize-lines
   .. rst:directive:option:: linenos

      Hier ein Beispiel aus unserem :doc:`jupyter-tutorial:index`:

      .. code-block:: rest

          .. literalinclude:: main.py
             :emphasize-lines: 3,7-10,20-22
             :linenos:

      .. literalinclude:: main.py
         :emphasize-lines: 3,7-10,20-22
         :linenos:
      
   .. rst:directive:option:: diff

      Wenn ihr das Diff eures Codes anzeigen möchtet, könnt ihr die alte Datei
      mit der ``diff``-Option angeben, :abbr:`z.B. (zum Beispiel)`:

      .. code-block:: rest

         .. literalinclude:: main.py
            :diff: main.py.orig

      .. literalinclude:: main.py
         :diff: main.py.orig

.. _deprecated:

Veralteter Code
---------------

.. rst:directive:: .. deprecated:: version

   Beschreibt, wann die Funktion veraltet wurde. Es kann auch eine Erklärung
   angegeben werden, um :abbr:`z.B.` darüber zu informieren, was stattdessen
   verwendet werden sollte. Beispiel:

   .. code-block:: rest

      .. deprecated:: 4.1
         verwende stattdessen :func:`new_function`.

   .. deprecated:: 4.1
      verwende stattdessen :func:`new_function`.

.. rst:directive:option:: py:module:deprecated

   Markiert ein Python-Modul als veraltet; es wird dann an verschiedenen
   Stellen als solches gekennzeichnet.
