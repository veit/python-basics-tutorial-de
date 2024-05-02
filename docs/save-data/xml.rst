Das ``xml``-Modul
=================

Das :doc:`XML <python3:library/xml>`-Modul wird mit Python mitgeliefert. Im
folgenden Abschnitt werden wir uns auf die zwei Untermodule :doc:`minidom
<python3:library/xml.dom.minidom>` und :doc:`ElementTree
<python3:library/xml.etree.elementtree>`.

Arbeiten mit ``minidom``
------------------------

Im folgenden Beispiel analysieren wir :download:`books.xml`:

.. literalinclude:: books.xml
   :language: xml
   :lines: 1-
   :lineno-start: 1

#. Hierzu impportieren wir zunächst das ``minidom``-Modul und geben ihm
   denselben Namen, damit es leichter referenziert werden kann:

   .. literalinclude:: minidom_example.py
      :language: py
      :lines: 1
      :lineno-start: 1

#. Anschließend definieren wir die Methode ``getTitles`` und erfassen mit der
   Methode ``getElementsByTagName`` die gewünschten XML-Tags:

   .. literalinclude:: minidom_example.py
      :language: py
      :lines: 4-10
      :lineno-start: 4

#. Dann erstellen wir eine leere Liste namens ``titles``, die mit den
   Titelobjekten gefüllt wird:

   .. literalinclude:: minidom_example.py
      :language: py
      :lines: 12-15
      :lineno-start: 12

#. Nun wird in verschachtelten ``for``-Schleifen der Titel ausgegeben:

   .. literalinclude:: minidom_example.py
      :language: py
      :lines: 17-21
      :lineno-start: 17

#. Schließlich setzen wir die ``__name__``-Variable noch wie ``__main__``
   gesetzt, sodass das Modul wie das Hauptprogramm ausgeführt werden kann.
   Anschließend wenden wir unsere ``getTitles``-Methode auf unsere
   :download:`books.xml`-Datei an:

   .. literalinclude:: minidom_example.py
      :language: py
      :lines: 24-
      :lineno-start: 24

Parsen mit ElementTree
----------------------

#. Importieren von ``cElementTree``:

   .. literalinclude:: elementtree_example.py
      :language: py
      :lines: 1
      :lineno-start: 1

   .. note::
      ``cElementTree`` ist in C geschrieben und ist erheblich schneller als
      ``ElementTree``.

#. Anschließend definieren wir die Methode ``parseXML`` und das Wurzelelement
   ``root``:

   .. literalinclude:: elementtree_example.py
      :language: py
      :lines: 4-11
      :lineno-start: 4

   .. code-block:: pycon

      <Element 'catalog' at 0x10b009620>
      tag=catalog, attrib={}

#. Ausgeben der XML-Kindelemente von ``book``:

   .. literalinclude:: elementtree_example.py
      :language: py
      :lines: 13-17
      :lineno-start: 13

   .. code-block:: pycon

      book {'id': '1'}
      title
      language
      author
      license
      date
      book {'id': '2'}
      ...

#. Inhalte der Kindelemente mit ``iter`` ausgeben:

   .. literalinclude:: elementtree_example.py
      :language: python
      :lines: 20-27
      :lineno-start: 20

   .. code-block:: pycon

      --------------------
      Iterating using iter
      --------------------
      catalog=
      book=
      title=Python basics
      language=en
      author=Veit Schiele
      license=BSD-3-Clause
      date=2021-10-28
      book=
      title=Jupyter Tutorial
      ...
