Platzhalter
-----------

Sphinx unterscheidet die folgenden Platzhaltervariablen:

.. rst:role:: envvar

    Umgebungsvariable, die auch einen Verweis auf die passende
    :rst:dir:`envvar`-Direktive erzeugt, falls sie existiert.

.. rst:role:: file

    Der Name einer Datei oder eines Verzeichnisses. Geschweifte Klammern können
    verwendet werden, um :abbr:`z.B. (zum Beispiel)` einen variablen Teil
    anzugeben::

        … is installed in :file:`/usr/lib/python3.{x}/site-packages` …

    In der generierten HTML-Dokumentation wird das ``x`` mit ``em .pre``
    besonders ausgezeichnet und kursiv dargestellt, um zu zeigen, dass es durch
    die spezifische Python-Version ersetzt werden soll.

.. rst:role:: makevar

    Der Name einer :command:`make`-Variable

.. rst:role:: samp

    Textbeispiel, wie :abbr:`z.B. (zum Beispiel)` Code, innerhalb dessen
    geschweifte Klammern verwendet werden können, um einen variablen Teil
    anzuzeigen, wie in :rst:role:`file` oder in ``:samp:`print 1+{VARIABLE}```.

    Ab Sphinx≥1.8 können geschweifte Klammern mit einem Backslash (``\``)
    angezeigt werden.

.. note::
    .. rst:role:: content

    Diese Rolle hat standardmäßig keine besondere Bedeutung. Ihr könnt sie daher
    für alles Mögliche zu verwenden, :abbr:`z.B. (zum Beispiel)` auch für
    Variablennamen.

.. seealso::
   * `Sphinx awesome sampdirective
     <https://github.com/kai687/sphinxawesome-sampdirective>`_
