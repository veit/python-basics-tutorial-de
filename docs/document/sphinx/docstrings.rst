Docstrings
==========

Mit der Sphinx-Erweiterung `sphinx.ext.autodoc
<https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_ können
Docstrings auch in die Dokumentation aufgenommen werden. Die folgenden
Direktiven können angegeben werden

… für Klassen und Ausnahmen:

.. rst:directive::  automodule
                    autoclass
                    autoexception

… für funktionsähnliche Objekte:

.. rst:directive::  autofunction
                    automethod
                    autoproperty
                    autodecorator

… für Daten und Attribute:

.. rst:directive::  autodata
                    autoattribute

Installation
------------

``sphinx.ext.autodoc`` ist normalerweise bereits in der
Sphinx-Konfigurationsdatei ``docs/conf.py`` angegeben:

.. code-block:: python

   extensions = ["sphinx.ext.autodoc", ...]

Wenn euer Paket und die zugehörige Dokumentation Teil desselben Repository
sind, werden sie immer dieselbe relative Position im Dateisystem haben. In
diesem Fall könnt ihr einfach die Sphinx-Konfiguration für ``sys.path``
bearbeiten, um den relativen Pfad zum Paket anzugeben, also:

.. code-block:: python

   sys.path.insert(0, os.path.abspath(".."))
   import MODULE

Wenn ihr eure Sphinx-Dokumentation in einer virtuellen Umgebung installiert
habt, könnt ihr euer Paket auch dort mitinstallieren, :abbr:`z.B. (zum
Beispiel)` indem ihr es in eure :file:`requirements.txt`-Datei eintragt.

Beispiele
---------

Hier findet ihr einige Beispiele aus der Dokumentation des
Python-:py:mod:`string`-Moduls:

.. literalinclude:: autodoc-examples.rst
   :language: rest
   :lines: 3-

Die Ausgabe ist :doc:`autodoc-examples`.

Richtlinien
-----------

In :pep:`8` gibt es nur einen kurzen Hinweise auf Konventionen für einen guten
Docstring: :pep:`Documentation Strings <8#comments>`. Weitere :abbr:`PEPs (Python Enhancement Proposals)` beziehen sich auf das :pep:`Docstring Processing
System Framework <256>`:

:pep:`257`
    beschreibt Docstring-Konventionen:

    * Was sollte wo dokumentiert werden?
    * Die erste Zeile soll eine einzeilige Zusammenfassung sein.

:pep:`258`
    spezifiziert die System zur Verarbeitung von Docstrings.

:pep:`287`
    spezifiziert die Docstring-Syntax.

Im `Google Python Style Guide
<https://google.github.io/styleguide/pyguide.html>`_ gibt es darüberhinaus noch
spezifischere Richtlinien für `Python Konnentare und Docstrings
<https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings>`_.
Auch der `NumPy Style guide
<https://numpydoc.readthedocs.io/en/latest/format.html>`_ liefert noch weitere
`Docstring-Standards <https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard>`_. Der wesentliche Unterschied zwischen beiden besteht darin,
dass Google Einrückungen verwendet und NumPy Unterstreichungen:

Google Python Style Guide:
    .. code-block:: python

       def func(arg1: int, arg2: str) -> bool:
           """Summary line.

           Extended description of function.

           Args:
               arg1 (int): Description of arg1
               arg2 (str): Description of arg2

           Returns:
               bool: Description of return value

           """
           return True

NumPy Style Guide:
    .. code-block:: python

       def func(arg1: int, arg2: str) -> bool:
           """Summary line.

           Extended description of function.

           Parameters
           ----------
           arg1 : int
               Description of arg1
           arg2 : str
               Description of arg2

           Returns
           -------
           bool
               Description of return value

           """
           return True

.. _napoleon:

``sphinx.ext.napoleon``
~~~~~~~~~~~~~~~~~~~~~~~

Die Sphinx-Erweiterung `sphinx.ext.napoleon
<https://sphinxcontrib-napoleon.readthedocs.io/>`_ verarbeitet sowohl
Docstrings, die dem Google Python Style Guide als auch dem NumPy Style Guide
entsprechen:

Detaillierte Konfigurationsoptionen findet ihr in `sphinxcontrib.napoleon.Config
<https://sphinxcontrib-napoleon.readthedocs.io/en/latest/sphinxcontrib.napoleon.html#sphinxcontrib.napoleon.Config>`_.

``sphinx-autodoc-typehints``
----------------------------

Mit :pep:`484` wurde eine Standardmethode für den Ausdruck von Typen in
Python-Code eingeführt. Damit können Typen auch in Docstrings unterschiedlich
ausgedrückt werden. Die Variante mit Typen nach PEP 484 hat den Vorteil, dass
Typ-Tester und IDEs zur statischen Codeanalyse eingesetzt werden können.

Python 3 Type-Annotations in Docstrings:
    .. code-block:: python

       def func(arg1: int, arg2: str) -> bool:
           """Summary line.

           Extended description of function.

           Args:
               arg1 (int): Description of arg1
               arg2 (str): Description of arg2

           Returns:
               bool: Description of return value

           """
           return True
