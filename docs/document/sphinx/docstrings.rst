Docstrings
==========

Mit der Sphinx-Erweiterung `sphinx.ext.autodoc
<https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_ können
Docstrings auch in die Dokumentation aufgenommen werden. Die folgenden drei
Direktiven können angegeben werden:

.. rst:directive::  automodule
                    autoclass
                    autoefunction

Diese dokumentieren ein Modul, eine Klasse oder eine Funktion unter Verwendung
des jeweiligen Docstrings.

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

.. note::
   Ihr solltet diese Richtlinien befolgen, wenn ihr Docstrings schreibt:

   * :pep:`8#comments`
   * :pep:`257#specification`

``sphinx-autodoc-typehints``
----------------------------

Mit :pep:`484` wurde eine Standardmethode für den Ausdruck von Typen in
Python-Code eingeführt. Damit können Typen auch in Docstrings unterschiedlich
ausgedrückt werden. Die Variante mit Typen nach PEP 484 hat den Vorteil, dass
Typtester und IDEs zur statischen Codeanalyse eingesetzt werden können.

Python 3 Type-Annotations:

    .. code-block:: python

        def func(arg1: int, arg2: str) -> bool:
            """Summary line.

            Extended description of function.

            Args:
                arg1: Description of arg1
                arg2: Description of arg2

            Returns:
                Description of return value

            """
            return True

Typen in Docstrings:

    .. code-block:: python

        def func(arg1, arg2):
            """Summary line.

            Extended description of function.

            Args:
                arg1 (int): Description of arg1
                arg2 (str): Description of arg2

            Returns:
                bool: Description of return value

            """
            return True

.. note::
   :pep:`484#suggested-syntax-for-python-2-7-and-straddling-code` are currently
   not supported by Sphinx and do not appear in the generated documentation.

.. _napoleon:

``sphinx.ext.napoleon``
-----------------------

Die Sphinx-Erweiterung `sphinx.ext.napoleon
<https://sphinxcontrib-napoleon.readthedocs.io/>`_ ermöglicht euch, verschiedene
Abschnitte in Docstrings zu definieren, einschließlich:

* ``Attributes``
* ``Example``
* ``Keyword Arguments``
* ``Methods``
* ``Parameters``
* ``Warning``
* ``Yield``

Es gibt zwei Arten von docstrings in ``sphinx.ext.napoleon``:

* `Google
  <https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html>`_
* `NumPy
  <https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html>`_

Der Hauptunterschied besteht darin, dass Google Einrückungen verwendet und NumPy
Unterstreichungen:

Google:

    .. code-block:: python

        def func(arg1, arg2):
            """Summary line.

            Extended description of function.

            Args:
                arg1 (int): Description of arg1
                arg2 (str): Description of arg2

            Returns:
                bool: Description of return value

            """
            return True

NumPy:

    .. code-block:: python

        def func(arg1, arg2):
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

Detaillierte Konfigurationsoptionen findet ihr in `sphinxcontrib.napoleon.Config
<https://sphinxcontrib-napoleon.readthedocs.io/en/latest/sphinxcontrib.napoleon.html#sphinxcontrib.napoleon.Config>`_.
