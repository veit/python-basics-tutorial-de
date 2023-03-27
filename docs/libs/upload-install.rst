Paket hochladen
===============

Schließlich könnt ihr das Paket auf dem :term:`Python Package Index (PyPI)`
oder einem anderen Index bereitstellen.

Hierfür müsst ihr euch bei *Test PyPI* registrieren. *Test-PyPI* ist eine
separate Instanz, die zum Testen und Experimentieren vorgesehen ist. Um dort
ein Konto einzurichten, geht ihr auf https://test.pypi.org/account/register/.
Weitere Informationen findet ihr unter `Using TestPyPI
<https://packaging.python.org/guides/using-testpypi/>`_.

Nun könnt ihr eine ``~/.pypirc``-Datei erstellen:

.. code-block:: ini

    [distutils]
    index-servers=
        test

    [test]
    repository = https://test.pypi.org/legacy/
    username = veit

.. seealso::
    Wenn ihr die PyPI-Anmeldung automatisieren wollt, lest bitte `Careful With
    That PyPI
    <https://glyph.twistedmatrix.com/2017/10/careful-with-that-pypi.html>`_.

Nachdem ihr registriert seid, könnt ihr euer :term:`Distribution Package` mit
:term:`twine` hochladen. Hierzu müsst ihr jedoch zunächst twine installieren
mit:

.. code-block:: console

    $ python -m pip install --upgrade pip build twine
    …
    All dependencies are now up-to-date!

.. note::
   Führt diesen Befehl vor jedem Release aus um sicherzustellen, dass alle
   Release-Tools auf dem neuesten Stand sind.

Nun könnt ihr eure :term:`Distribution Packages <Distribution Package>`
erstellen mit:

.. code-block:: console

    $ cd /path/to/your/distribution_package
    $ rm -rf build dist
    $ pyproject-build .

Nach der Installation von Twine könnt ihr alle Archive unter ``/dist`` auf den
Python Package Index hochladen mit:

.. code-block:: console

    $ twine upload -r test -s dist/*

``-r``, ``--repository``
    Das Repository zum Hochladen des Pakets.

    In unserem Fall wird ``test``-Abschnitt aus der ``~/.pypirc``-Datei
    verwendet.

``-s``, ``--sign``
    signiert die hochzuladenden Dateien mit GPG.

Ihr werdet nach eurem Passwort gefragt, mit dem ihr euch bei *Test PyPI*
registriert habt. Anschließend solltet ihr eine ähnliche Ausgabe sehen:

.. code-block:: console

    Uploading distributions to https://test.pypi.org/legacy/
    Enter your username: veit
    Enter your password:
    Uploading example-0.0.1-py3-none-any.whl
    100%|█████████████████████| 4.65k/4.65k [00:01<00:00, 2.88kB/s]
    Uploading example-0.0.1.tar.gz
    100%|█████████████████████| 4.25k/4.25k [00:01<00:00, 3.05kB/s]

.. note::
   Wenn ihr eine ähnliche Fehlermeldung erhaltet wie

   .. code-block:: console

    The user 'veit' isn't allowed to upload to project 'example'

   müsst ihr einen eindeutigen Namen für euer Paket auswählen:

   #. ändert das ``name``-Argument in der ``setup.py``-Datei
   #. entfernt das ``dist``-Verzeichnis
   #. generiert die Archive neu

Überprüfen
----------

Installation
~~~~~~~~~~~~

Ihr könnt :term:`pip` verwenden um euer Paket zu installieren und zu überprüfen,
ob es funktioniert. Erstellt eine neue :term:`virtuelle Umgebung` und
installiert euer Paket von *Test PyPI*:

.. code-block:: console

    $ mkdir test_env
    $ cd !$
    cd test_env
    $ source bin/activate
    $ python -m pip install --extra-index-url https://test.pypi.org/simple/ minimal_example

.. note::
   Wenn ihr einen anderen Paketnamen verwendet habt, ersetzt ihn im obigen
   Befehl durch euren Paketnamen.

:term:`pip` sollte das Paket von *Test PyPI* installieren und die Ausgabe sollte
in etwa so aussehen:

.. code-block:: console

    Collecting example_pkg
      Downloading https://test-files.pythonhosted.org/packages/.../minimal_example-0.0.1-py3-none-any.whl
    Installing collected packages: minimal_example
    Successfully installed minimal_example-0.0.1

Ihr könnt testen, ob euer Paket korrekt installiert wurde indem ihr das Modul
importiert und auf die ``name``-Eigenschaft referenziert, die zuvor in
``__init__.py`` eingegeben wurde:

.. code-block:: console

    $ python
    Python 3.7.0 (default, Aug 22 2018, 15:22:29)
    …
    >>> import minimal_example
    >>> minimal_example.name
    'minimal_example'

.. note::

    Die Pakete auf *Test-PyPI* werden nur temporär gespeichert. Wenn ihr ein
    Paket in den echten :term:`Python Package Index (PyPI)` hochladen wollt,
    könnt ihr dies tun, indem ihr ein Konto auf https://pypi.org anlegt und die
    gleichen Anweisungen befolgt, jedoch ``twine upload dist/*`` verwendet.

README
~~~~~~

Überprüft bitte auch, ob die ``README.rst``-Datei auf der Test-PyPI-Seite
korrekt angezeigt wird.

PyPI
----

Registriert euch nun beim :term:`Python Package Index (PyPI)` und stellt sicher,
dass die `Zwei-Faktor-Authentifizierung
<https://blog.python.org/2019/05/use-two-factor-auth-to-improve-your.html>`_
aktiviert ist indem ihr die ``~/.pypirc``-Datei ergänzt:

.. code-block:: ini

    [distutils]
    index-servers=
        pypi
        test

    [test]
    repository = https://test.pypi.org/legacy/
    username = veit

    [pypi]
    username = __token__

Mit dieser Konfiguration wird nicht mehr die Name/Passwort-Kombination beim
Hochladen verwendet sondern ein Upload-Token.

.. seealso::
    * `PyPI now supports uploading via API token
      <https://pyfound.blogspot.com/2019/07/pypi-now-supports-uploading-via-api.html>`_
    * `What is two factor authentication and how does it work on PyPI?
      <https://pypi.org/help/#twofa>`_

Schließlich könnt ihr nun euer Paket auf PyPI veröffentlichen:

.. code-block:: console

    $ twine upload -r pypi -s dist/*

.. note::
    Ihr könnt Releases nicht einfach ersetzen da ihr Pakete mit derselben
    Versionsnummer nicht erneut hochladen könnt.

.. note::
   Entfernt nicht alte Versionen aus dem Python Package Index. Dies verursacht
   nur Arbeit für jene, die diese Version weiter verwenden wollen und dann auf
   alte Versionen auf GitHub ausweichen müssen. PyPI hat eine `yank
   <https://pypi.org/help/#yanked>`_-Funktion, die ihr stattdessen nutzen
   könnt. Dies ignoriert eine bestimmte Version, wenn sie nicht explizit mit
   ``==`` oder ``===`` explizit angegeben wurde.

.. seealso::
    * `PyPI Release Checklist
      <https://cookiecutter-namespace-template.readthedocs.io/en/latest/pypi-release-checklist.html>`_

GitHub Action
-------------

Ihr könnt auch eine GitHub-Aktion erstellen, die ein Paket erstellt und auf PyPI
hochlädt. Eine solche ``.github/workflows/pypi.yml``-Datei könnte folgendermaßen
aussehen:

.. code-block:: yaml

    name: pypi
    on:
      push:
        tags:
        - '*'

    jobs:
      package-and-deploy:

        runs-on: ubuntu-latest

        steps:
          - name: Checkout
            uses: actions/checkout@v2
            with:
              fetch-depth: 0

          - name: Set up Python
            uses: actions/setup-python@v2
            with:
              python-version: 3.8

          - name: Install dependencies
            run: |
              python -m pip install -U pip
              python -m pip install -U setuptools twine wheel

          - name: Build and publish
            env:
              TWINE_PASSWORD: ${{ secrets.TWINE_PASSWORD }}
              TWINE_USERNAME: ${{ secrets.TWINE_USERNAME }}
            run: |
              python setup.py sdist bdist_wheel
              twine upload dist/*

.. seealso::

   * `GitHub Actions <https://docs.github.com/en/actions>`_
   * :doc:`cibuildwheel`
