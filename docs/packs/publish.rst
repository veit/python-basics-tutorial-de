Paket veröffentlichen
=====================

Schließlich könnt ihr das Paket auf dem :term:`Python Package Index`
(:term:`PyPI`) oder einem anderen Index, :abbr:`z.B. (zum Beispiel)`
:doc:`gitlab` oder :term:`devpi`, bereitstellen.

Für den :term:`Python Package Index` müsst ihr euch bei *Test PyPI*
registrieren. *Test-PyPI* ist eine separate Instanz, die zum Testen und
Experimentieren vorgesehen ist. Um dort ein Konto einzurichten, geht ihr auf
https://test.pypi.org/account/register/. Weitere Informationen findet ihr unter
`Using TestPyPI
<https://packaging.python.org/en/latest/guides/using-testpypi/>`_.

Nun könnt ihr eine :file:`~/.config/pip/pip.conf`-Datei erstellen:

.. code-block:: ini

    [distutils]
    index-servers=
        test

    [test]
    repository = https://test.pypi.org/legacy/
    username = veit

.. seealso::
   Wenn ihr die PyPI-Anmeldung automatisieren wollt, lest zunächst `Careful
   With That PyPI
   <https://glyph.twistedmatrix.com/2017/10/careful-with-that-pypi.html>`_.

Nachdem ihr registriert seid, könnt ihr euer :term:`Distribution Package` mit
``uv publish`` hochladen.

Dabei könnt ihr ``uv publish`` entweder mit der Option ``--username __token__``
verwenden oder die Umgebungsvariable ``UV_PUBLISH_USERNAME=__token__`` setzen,
um alle Archive unter :file:`/dist` auf den :term:`Python Package Index`
hochladen:

.. code-block:: console

    $ uv publish --publish-url https://test.pypi.org/legacy/ --username __token__ dist/*

``--publish-url``
    Die URL des Upload-Endpunkts (nicht die Index-URL).

``--username``
    Den Benutzernamen für den Upload.

.. note::
   Wenn ihr eine ähnliche Fehlermeldung erhaltet wie

   .. code-block:: console

      The user 'veit' isn't allowed to upload to project 'example'

   müsst ihr einen eindeutigen Namen für euer Paket auswählen:

   #. ändert das ``name``-Argument in der :file:`pyproject.toml.`-Datei
   #. entfernt das ``dist``-Verzeichnis
   #. generiert die Archive neu

Überprüfen
----------

Installation
~~~~~~~~~~~~

Ihr könnt ``uv`` verwenden um euer Paket von *Test PyPI* zu installieren und zu
überprüfen, ob es funktioniert:

.. code-block:: console

    uv add -i https://test.pypi.org/simple/ mypack

.. note::
   Wenn ihr einen anderen Paketnamen verwendet habt als ``mypack``, ersetzt ihn
   im obigen Befehl durch euren Paketnamen.

``uv add`` sollte das Paket von *Test PyPI* installieren und die Ausgabe sollte
in etwa so aussehen:

.. code-block:: console

   Resolved 8 packages in 5ms
   Installed 7 packages in 36ms
    + mypack==0.1.0

Ihr könnt testen, ob euer Paket korrekt installiert wurde indem ihr :func:`main`
aufruft:

.. code-block:: console

   $ uv run mypack
   Hello from mypack!

.. note::

    Die Pakete auf *Test-PyPI* werden nur temporär gespeichert. Wenn ihr ein
    Paket in den echten :term:`Python Package Index` (:term:`PyPI`) hochladen
    wollt, könnt ihr dies tun, indem ihr ein Konto auf :term:`pypi.org` anlegt.

README
~~~~~~

Überprüft auch, ob die :file:`README.rst`-Datei auf der Test-PyPI-Seite korrekt
angezeigt wird.

PyPI
----

Registriert euch nun beim :term:`Python Package Index` (:term:`PyPI`) und stellt
sicher, dass die `Zwei-Faktor-Authentifizierung
<https://blog.python.org/2019/05/use-two-factor-auth-to-improve-your.html>`_
aktiviert ist, indem ihr Folgendes in die Datei :file:`~/.pypirc` einfügt:

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

Bei dieser Konfiguration wird für das Hochladen nicht mehr die Kombination aus
Name und Passwort verwendet, sondern ein Upload-Token.

.. seealso::
    * `PyPI now supports uploading via API token
      <https://pyfound.blogspot.com/2019/07/pypi-now-supports-uploading-via-api.html>`_
    * `What is two factor authentication and how does it work on PyPI?
      <https://pypi.org/help/#twofa>`_

Schließlich könnt ihr nun euer Paket auf :term:`PyPI` veröffentlichen:

.. code-block:: console

    $ uv publish dist/*

.. note::
    Ihr könnt Releases nicht einfach ersetzen da ihr Pakete mit derselben
    Versionsnummer nicht erneut hochladen könnt.

.. note::
   Entfernt nicht alte Versionen aus dem Python Package Index. Dies verursacht
   nur Arbeit für jene, die diese Version weiter verwenden wollen und dann auf
   alte Versionen auf GitHub ausweichen müssen. PyPI hat eine `yank
   <https://pypi.org/help/#yanked>`_-Funktion, die ihr stattdessen nutzen
   könnt. Dies ignoriert eine bestimmte Version, wenn sie nicht explizit mit
   ``==`` oder ``===`` angegeben wurde.

.. seealso::
   * `PyPI Release Checklist
     <https://cookiecutter-namespace-template.readthedocs.io/en/latest/pypi-release-checklist.html>`_

.. _pypi_github_action:

GitHub Action
-------------

Ihr könnt auch eine GitHub-Aktion erstellen, die ein Paket erstellt und auf PyPI
hochlädt. Eine solche :file:`.github/workflows/pypi.yml`-Datei könnte
folgendermaßen aussehen:

.. code-block:: yaml
   :caption: .github/workflows/pypi.yml
   :linenos:
   :emphasize-lines: 3-5, 12, 31, 36, 38-

   name: Publish Python Package

    on:
      release:
        types: [created]

   jobs:
     test:
       …
     package-and-deploy:
       runs-on: ubuntu-latest
       needs: [test]
       steps:
       - name: Checkout
         uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
         with:
           fetch-depth: 0
       - name: Set up Python
         uses: actions/setup-python@a309ff8b426b58ec0e2a45f0f869d46889d02405 # v6.2.0
         with:
           python-version-file: .python-version
           cache-dependency-path: '**/pyproject.toml'
       - name: Setup cached uv
         uses: hynek/setup-cached-uv@4300ec2180bc77d705e626a34e381b81a4772c51 # v2.5.0
       - name: Create venv
         run: |
           uv venv
           echo "$PWD/.venv/bin" >> $GITHUB_PATH
       - name: Build
         run: |
           uv build
       - name: Retrieve and publish
         steps:
         - name: Retrieve release distributions
           uses: actions/download-artifact@3e5f45b2cfb9172054b4087a40e8e0b5a5461e7c # v8.0.1
         - name: Publish package distributions to PyPI
           uses: pypa/gh-action-pypi-publish@ed0c53931b1dc9bd32cbe73a98c7f6766f8a527e # v1.13.0
           with:
             username: __token__
             password: ${{ secrets.PYPI_TOKEN }}

Zeilen 3–5
    Dies stellt sicher, dass der Arbeitsablauf jedes Mal ausgeführt wird, wenn
    ein neues GitHub-Release für das Repository erstellt wird.
Zeile 12
    Der Job wartet auf das Bestehen des ``test``-Jobs bevor er ausgeführt wird.
Zeile 31
    Hier sollte :samp:`{mypack}` durch euren Paketnamen ersetzt werden.
Zeile 36
    Die GitHub-Aktion ``actions/download-artifact`` stellt die gebauten
    Verteilungspakete bereit.
Zeile 38–41
    Die GitHub-Aktion ``pypa/gh-action-pypi-publish`` veröffentlicht die Pakete
    mit dem Upload-Token auf :term:`PyPI`.

.. seealso::

   * `GitHub Actions <https://docs.github.com/en/actions>`_
   * :doc:`cibuildwheel`

Absichern des Release-Workflows
-------------------------------

Continuous Deployment zur Veröffentlichung von Python-Paketen sind ein beliebtes
Ziel für Angriffe. Ihr könnt viele der Gefahren vermeiden, indem ihr einige
wenige Sicherheitsempfehlungen befolgt:

Vermeidet unsichere Trigger
    Workflows, die bei einem Angriff ausgelöst werden können, insbesondere
    Eingaben, die dieser kontrolliert (wie :ref:`Pull Requests
    <merge-pull-requests>`- oder :doc:`Branch
    <Python4DataScience:productive/git/branch>`-Titel), wurden in der
    Vergangenheit genutzt, Befehle einzuschleusen. Insbesondere der Trigger
    ``pull_request_target`` der :ref:`github-actions` sollte  vermieden werden.
Säubert Parameter und Eingaben
    Jeder Workflow-Parameter oder jede Eingabe, die sich zu einem ausführbaren
    Befehl erweitern lässt, birgt das Potenzial, bei Angriffen ausgenutzt zu
    werden. Säubert Werte, indem ihr sie als Umgebungsvariablen an Befehle
    übergebt, um :abbr:`SSTI (Server Side Template Injection)`-Angriffe zu
    vermeiden.
Vermeidet veränderbare Referenzen
    fixiert eure Abhängigkeiten in Workflows.

    * Bevorzugt Git-Commit-`SHA
      <https://de.wikipedia.org/wiki/Secure_Hash_Algorithm>`_-Werte anstelle von
      :doc:`Git-Tags <Python4DataScience:productive/git/tag>`, da Tags änderbar
      sind.
    * Verwendet eine :ref:`uv_lock` für PyPI-Abhängigkeiten, die in Workflows
      verwendet wird.

Verwendet überprüfbare Deployments
    Mit :ref:`trusted_publishers` könnt ihr überprüfbare GitHub-Umgebungen für
    den Bau eurer Python-Pakete verwenden. Falls ihr GitHub Actions für die
    kontinuierliche Bereitstellung nutzt, solltet ihr :ref:`zizmorcore`
    verwenden um unsichere Workflows zu erkennen und zu beheben.

.. _trusted_publishers:

Trusted Publishers
~~~~~~~~~~~~~~~~~~

`Trusted Publishers <https://docs.pypi.org/trusted-publishers/>`_ ist ein
Verfahren zum Veröffentlichen von Paketen auf dem :term:`PyPI`. Es  basiert auf
OpenID Connect und erfordert weder Passwort noch Token. Dazu sind lediglich die
folgenden Schritte erforderlich:

#. Fügt einen *Trusted Publisher* auf PyPI hinzu

   Je nachdem, ob ihr ein neues Paket veröffentlichen oder ein bestehendes
   aktualisieren wollt, unterscheidet sich der Prozess geringfügig:

   * zum Aktualisieren eines bestehenden Pakets siehe `Adding a trusted
     publisher to an existing PyPI project
     <https://docs.pypi.org/trusted-publishers/adding-a-publisher/>`_
   * zum veröffentlichen eines neuen Pakets gibt es ein spezielles Verfahren,
     *Pending Publisher* genannt; :abbr:`s.a. (siehe auch)` `Creating a PyPI
     project with a trusted publisher
     <https://docs.pypi.org/trusted-publishers/creating-a-project-through-oidc/>`_

     Ihr könnt damit auch einen Paketnamen reservieren, bevor ihr die erste
     Version veröffentlicht. Damit könnt ihr sicherstellen, dass ihr das Paket
     auch unter dem gewünschten Namen veröffentlichen könnt.

     Hierfür müsst ihr in `pypi.org/manage/account/publishing/
     <https://pypi.org/manage/account/publishing/>`_ einen neuen *Pending
     Publisher* erstellen mit

     * Namen des PyPI-Projekts
     * GitHub-Repository Owner
     * Namen des Workflows, :abbr:`z.B. (zum Beispiel)` :file:`publish.yml`
     * Name der Umgebung (optional), :abbr:`z.B. (zum Beispiel)` ``release``

#. Erstellt eine Umgebung für die GitHub-Actions

   Wenn wir eine Umgebung auf :term:`PyPI` angegeben haben, müssen wir diese nun
   auch erstellen. Das kann in :menuselection:`Settings --> Environments` für
   das Repository geschehen. Der Name unserer Umgebung ist ``release``.

#. Konfiguriert den Arbeitsablauf

   Hierfür erstellen wir nun die Datei :file:`.github/workflows/publish.yml` in
   unserem Repository:

   .. code-block:: yaml
      :caption: .github/workflows/pypi.yml
      :lineno-start: 10
      :emphasize-lines: 3, 4-5

      package-and-deploy:
          runs-on: ubuntu-latest
      +   environment: release
      +   permissions:
      +     id-token: write
          needs: [test]
          steps:

   Zeile 12
       Die Angabe einer GitHub-Umgebung ist optional, wird aber dringend
       empfohlen.
   Zeilen 13–14
       Die ``write``-Berechtigung ist für *Trusted Publishing* erforderlich.

   Zeilen 40–44
       ``username`` und ``password`` werden für die GitHub-Aktion
       ``pypa/gh-action-pypi-publish`` nicht mehr benötigt.

       .. code-block:: yaml
          :caption: .github/workflows/pypi.yml
          :lineno-start: 40
          :emphasize-lines: 3-

          - name: Publish package distributions to PyPI
            uses: pypa/gh-action-pypi-publish@ed0c53931b1dc9bd32cbe73a98c7f6766f8a527e # v1.13.0
            with:
              username: __token__
              password: ${{ secrets.PYPI_TOKEN }}

Seit 14. November 2024 unterstützt :term:`PyPI` auch :pep:`740` mit `Digital
Attestations <https://docs.pypi.org/attestations/>`_. PyPI verwendet das
`in-toto Attestation Framework <https://github.com/in-toto/attestation>`_ zum
Ausstellen der Digital Attestations `SLSA Provenance
<https://slsa.dev/spec/v1.0/provenance>`_ und `PyPI Publish Attestation (v1)
<https://docs.pypi.org/attestations/publish/v1/>`_.

Die Erstellung und Veröffentlichung erfolgt standardmäßig, sofern über
:ref:`Trusted Publishing <trusted_publishers>` und die GitHub-Action
`pypa/gh-action-pypi-publish <https://github.com/pypa/gh-action-pypi-publish>`_
zum Veröffentlichen verwendet werden:

.. code-block:: yaml
   :caption: .github/workflows/pypi.yml

   jobs:
     pypi-publish:
       name: Upload release to PyPI
       runs-on: ubuntu-latest
       environment:
         name: pypi
         url: https://pypi.org/p/{YOUR-PYPI-PROJECT-NAME}
       permissions:
         id-token: write
       steps:
       - name: Publish package distributions to PyPI
         uses: pypa/gh-action-pypi-publish@ed0c53931b1dc9bd32cbe73a98c7f6766f8a527e # v1.13.0

.. note::
   Die Unterstützung für die automatische Erstellung von Digital Attestations
   und die Veröffentlichung aus anderen Trusted Publisher-Umgebungen ist
   geplant.

.. seealso::
   `PyPI now supports digital attestations
   <https://blog.pypi.org/posts/2024-11-14-pypi-now-supports-digital-attestations/>`_

.. _zizmorcore:

zizmor
~~~~~~

`zizmor <https://docs.zizmor.sh>`_ kann viele Sicherheitsprobleme in typischen
CI/CD-Konfigurationen von GitHub Actions aufspüren und beheben. zizmor ist für
die Integration in GitHub Actions konzipiert. Eine typische GitHub Action von
uns für zizmor sieht so aus:

.. code-block:: yaml
   :caption: .github/workflows/zizmor.yml

   # https://github.com/woodruffw/zizmor
   name: Zizmor

   on:
     push:
       branches: ["main"]
     pull_request:
       branches: ["**"]

   concurrency:
     group: ${{ github.workflow }}-${{ github.event.pull_request.number || github.ref }}
     cancel-in-progress: true

   permissions: {}

   jobs:
     zizmor:
       name: Run zizmor
       runs-on: ubuntu-latest
       permissions:
         security-events: write # Required for upload-sarif (used by zizmor-action) to upload SARIF files.
       steps:
         - name: Checkout repository
           uses: actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd # v6.0.2
           with:
             persist-credentials: false
         - name: Run zizmor
           uses: zizmorcore/zizmor-action@71321a20a9ded102f6e9ce5718a2fcec2c4f70d8 # v0.5.2
           with:
             persona: pedantic

.. _add_2fa:

2FA für alle Entwicklungskonten
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ihr solltet Zwei-Faktor-Authentifizierung für alle eure Konten nutzen, die mit
der Entwicklung in Verbindung stehen – nicht nur für :term:`PyPI`. Denkt an eure
Konten für die Versionskontrolle (`GitHub <https://github.com/>`_, `GitLab
<https://about.gitlab.com/>`_, `Codeberg <https://codeberg.org/>`_, `Forgejo
<https://forgejo.org/>`_) und E-Mail.
