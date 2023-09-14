cruft
=====

Ein Problem mit cookiecutter-Vorlagen besteht darin, dass Projekte, die auf
älteren Versionen der Vorlage basieren, veralten, wenn sich nur die Vorlage im
Laufe der Zeit den sich ändeernden Anforderungen angepasst wird. `cruft <https://cruft.github.io/cruft/>`_ versucht, die Übernahme von Änderungen im
Git-Repository des :doc:`Cookiecutter-Templates <templates>` in daraus
abgeleitete Projekte zu vereinfachen.

Die wesentlichen Features von cruft sind:

* Mit ``cruft check`` könnt ihr schnell überprüfen, ob ein Projekt die neueste
  Version einer Vorlage verwendet. Diese Prüfung kann auch leicht in
  CI-Pipelines integriert werden, um sicherzustellen, dass eure Projekte
  synchron sind.
* cruft automatisiert auch die Aktualisierung der Projekte aus den
  cookiecutter-Vorlagen.

Installation
------------

.. code-block:: console

    $ python3.8 -m pip install cruft

Ein neues Projekt erstellen
---------------------------

Um ein neues Projekt mit cruft zu erstellen, könnt ihr :samp:`cruft create
{PROJECT_URL}` auf der Kommandozeile ausführen, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: console

    $ cruft create https://github.com/veit/cookiecutter-namespace-template
    full_name [Veit Schiele]:
    …

cruft verwendet dabei :doc:`Cookiecutter <features>` und der einzige Unterschied
in der resultierenden Ausgabe ist eine :file:`.cruft.json`-Datei, die den
Git-Hash der verwendeten Vorlage sowie die angegebenen Parameter enthält.

.. tip::

    Bestimmte Dateien eignen sich selten zum Aktualisieren, :abbr:`z.B. (Zum
    Beispiel)` Testfälle oder :file:`__init__`-Dateien. Ihr könnt cruft
    anweisen, die Aktualisierung dieser Dateien in einem Projekt immer zu
    überspringen, indem ihr das Projekt mit den Argumenten
    :samp:`--skip vsc__init__.py --skip tests` erzeugt oder sie manuell zu
    einem Skip-Abschnitt in eurer :file:`.cruft.json`-Datei hinzufügt:

    .. code-block:: javascript
        :emphasize-lines: 4-7

        {
          "template": "https://github.com/veit/cookiecutter-namespace-template",
          "commit": "521d4b2aa603aec186cd7e542295edb458ba4552",
          "skip": [
              "vsc/__init__.py",
              "tests"
          ],
          "checkout": null,
          "context": {
            "cookiecutter": {
              "full_name": "Veit Schiele",
              ...
            }
          },
          "directory": null
        }

Ein Projekt aktualisieren
-------------------------

Um ein bestehendes Projekt zu aktualisieren, das mit cruft erstellt wurde, könnt
ihr ``cruft update`` im Stammverzeichnis des Projekts ausführen. Wenn es
Aktualisierungen gibt, wird cruft euch zunächst bitten, diese zu überprüfen.
Wenn ihr die Änderungen akzeptiert, wird cruft sie auf euer Projekt anwenden und
die Datei :file:`.cruft.json` aktualisieren.

Ein Projekt überprüfen
----------------------

Um festzustellen, ob ein Projekt eine Vorlagenaktualisierung verpasst hat, könnt
ihr ganz einfach, ``cruft check`` aufrufen. Wenn das Projekt veraltet ist, wird
ein Fehler und der :samp:`Exit-Code 1` zurückgegeben. ``cruft check`` kann auch
zu :doc:`Python4DataScience:productive/git/hooks/pre-commit` und CI-Pipelines
hinzugefügt werden, um sicherzustellen, dass Projekte nicht ungewollt veralten.

Ein bestehendes Projekt verknüpfen
----------------------------------

Wenn ihr ein bestehendes Projekt habt, das ihr in der Vergangenheit mit
Cookiecutter direkt aus einer Vorlage erstellt habt, könnt ihr es mit
:samp:`cruft link {TEMPLATE_REPOSITORY}` mit der Vorlage verknüpfen, mit der es
erstellt wurde, :abbr:`z.B. (zum Beispiel)`:

.. code-block:: console

    $ cruft link https://github.com/veit/cookiecutter-namespace-template

Ihr könnt dann den letzten Commit der Vorlage angeben, mit dem das Projekt
aktualisiert wurde, oder die Vorgabe akzeptieren, den letzten Commit zu
verwenden.

Diff anzeigen
-------------

Mit der Zeit kann sich euer Projekt stark von der eigentlichen
Cookiecutter-Vorlage unterscheiden. ``cruft diff`` ermöglicht euch, schnell zu
sehen, was sich in Ihrem lokalen Projekt im Vergleich zur Vorlage geändert hat.
