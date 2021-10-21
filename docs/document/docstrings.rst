Docstrings
==========

Mit der Sphinx-Erweiterung `sphinx.ext.autodoc
<https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html>`_ können
Docstrings auch in die Dokumentation aufgenommen werden. Die folgenden drei
Direktiven können angegeben werden:

.. rst:directive::  automodule
                    autoclass
                    autoexception

Diese dokumentieren ein Modul, eine Klasse oder eine Ausnahme unter Verwendung
des docstring des jeweiligen Objekts.

Installation
------------

``sphinx.ext.autodoc`` ist normalerweise bereits in der
Sphinx-Konfigurationsdatei ``docs/conf.py`` angegeben:

.. code-block:: python

    extensions = [
        'sphinx.ext.autodoc',
        ...
    ]

Wenn euer Paket und die zugehörige Dokumentation Teil desselben Repositorys
sind, werden sie immer dieselbe relative Position im Dateisystem haben. In
diesem Fall könnt ihr einfach die Sphinx-Konfiguration für ``sys.path``
bearbeiten, um den relativen Pfad zum Paket anzugeben, also:

.. code-block:: python

    sys.path.insert(0, os.path.abspath('..'))
    import requests

Wenn ihr eure Sphinx-Dokumentation in einer virtuellen Umgebung installiert
habt, könnt ihr euer Paket auch dort mitinstallieren:

.. code-block:: console

    $ python -m pip install my.package

oder, wenn ihr das Paket weiterentwickeln wollt, mit:

.. code-block:: console

    $ python -m pip install -e https://github.com/veit/my.package.git

Beispiele
---------

Hier findet ihr einige Beispiele aus der API-Dokumentation für das `requests
<https://docs.python-requests.org>`_-Modul:

.. literalinclude:: docstrings-example.rst
   :language: rest
   :lines: 3-

Dies führt zum :doc:`docstrings-example`, die aus den folgenden Docstrings
generiert wird:

* `requests.head <https://docs.python-requests.org/en/master/_modules/requests/api/#head>`_
* `requests.RequestException <https://docs.python-requests.org/en/master/_modules/requests/exceptions/#RequestException>`_
* `requests.Session <https://docs.python-requests.org/en/master/_modules/requests/sessions/#Session>`_

.. autoclass:: Session
   :inherited-members:

.. note::
   Ihr solltet diese Richtlinien befolgen, wenn ihr Docstrings schreibt:

   * `Python Style Guide: comments
     <https://www.python.org/dev/peps/pep-0008/#comments>`_
   * `The Docstring Conventions Guide
     <https://www.python.org/dev/peps/pep-0257/#specification>`_

``sphinx-autodoc-typehints``
----------------------------

Mit  `PEP 484 <https://www.python.org/dev/peps/pep-0484/>`_ wurde eine
Standardmethode für den Ausdruck von Typen in Python-Code eingeführt. Damit
können Typen auch in Docstrings unterschiedlich ausgedrückt werden. Die Variante
mit Typen nach PEP 484 hat den Vorteil, dass Typtester und IDEs zur statischen
Codeanalyse eingesetzt werden können.

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
   `Python 2/3 compatible annotations
   <https://www.python.org/dev/peps/pep-0484/#suggested-syntax-for-python-2-7-and-straddling-code>`_
   are currently not supported by Sphinx and do not appear in the generated
   documentation.

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
Unterstriche:

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
