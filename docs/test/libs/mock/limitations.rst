Grenzen des Mocking
===================

Eines der größten Probleme bei der Verwendung von Mocks besteht darin, dass wir
bei in einem Test nicht mehr das Verhalten, sondern die Implementierung testen.
Dies ist jedoch nicht nur zeitaufwändig, sondern auch gefährlich: Ein gültiges
Refactoring :abbr:`z.B. (zum Beispiel)` das Ändern eines Variablennamens, kann
Tests zum Scheitern bringen, wenn diese bestimmte Variable gemockt wurde. Wir
wollen jedoch, dass unsere Tests nur dann fehlschlagen, wenn es Brüche im
Verhalten gibt, nicht jedoch nur bei Codeänderungen.

Manchmal ist Mocking jedoch der einfachste Weg, Exceptions oder
Fehlerbedingungen zu erzeugen und sicherzustellen, dass euer Code diese korrekt
behandelt. Es gibt auch Fälle, in denen das Testen von Verhalten unzumutbar ist,
wie :abbr:`z.B. (zum Beispiel)` beim Zugriff auf eine Zahlungs-API oder beim
Senden von E-Mails. In diesen Fällen ist es eine gute Option zu testen, ob euer
Code eine bestimmte API-Methode zum richtigen Zeitpunkt und mit den richtigen
Parametern aufruft.

.. seealso::
   * Hynek Schlawack: `“Don’t Mock What You Don’t Own”
     <https://hynek.me/articles/what-to-mock-in-5-mins/>`_

.. note::
   Auch bei agentischer Software-Entwicklung versuchen wir, Mocking so weit wie
   möglich zu vermeiden:

   .. code-block:: md
      :caption: AGENTS.md

      - Prefer testing real code where possible. Use mocks and `monkeypatch` when absolute necessary. Try to avoid mocking as much as possible.

   .. seealso::
      * :ref:`agentic-software-engineering:testing`

Mocking vermeiden mit Tests auf mehreren Ebenen
-----------------------------------------------

Wir können die Tasks-CLI auch ohne Mocks testen indem wir auch die API
verwenden. Dabei werden wir nicht die API testen, sondern sie nur verwenden, um
das Verhalten von Aktionen zu überprüfen, die über die CLI ausgeführt werden.
Das Beispiel :ref:`test_add_with_owner <test_add_with_owner>` können wir auch
folgendermaßen testen:

.. code-block:: python

   def test_add_with_owner(tasks_db, tasks_cli):
       tasks_cli("add some task -o veit")
       expected = tasks.Task("some task", owner="veit", state="todo")
       all = tasks_db.list_tasks()
       assert len(all) == 1
       assert all[0] == expected

Mocking testet die Implementierung der Befehlszeilenschnittstelle und stellt
sicher, dass ein API-Aufruf mit bestimmten Parametern erfolgt. Beim
Mixed-Layer-Ansatz wird das Verhalten getestet, um sicherzustellen, dass das
Ergebnis unseren Vorstellungen entspricht. Diese Ansatz ist viel weniger
ein Change-Detector und hat eine größere Chance, während eines Refactorings
gültig zu bleiben. Interessanterweise sind die Tests auch etwa doppelt so
schnell:

.. code-block:: pytest

   $ uv run pytest -s tests/cli/test_add.py::test_add_with_owner
   ============================= test session starts ==============================
   …
   configfile: pyproject.toml
   plugins: cov-4.1.0, Faker-19.11.0
   collected 1 item

   tests/cli/test_add.py .

   ============================== 1 passed in 0.03s ===============================

Wir könnten Mocking auch auf eine andere Weise vermeiden. Wir könnten das
Verhalten vollständig über die CLI testen. Dazu müsste möglicherweise die
Ausgabe der Tasks-Liste geparst werden, um den korrekten Datenbankinhalt zu
überprüfen.

In der API gibt :func:`add_task` einen Index zurück und bietet eine
:func:`get_task(index)`-Methode, die beim Testen hilft. Beide Methoden sind in
der CLI nicht vorhanden, könnten es aber sein. Wir könnten vielleicht die
Befehle ``tasks get index`` oder ``tasks info index`` hinzufügen, damit wir ein
Task abrufen können, anstatt ``tasks list für`` alles verwenden zu müssen.
``list`` unterstützt auch bereits Filterung. Vielleicht würde das Filtern nach
``index`` funktionieren, anstatt einen neuen Befehl hinzuzufügen. Und wir
könnten ``tasks add`` eine Ausgabe hinzufügen, die etwas sagt wie *Task
hinzugefügt bei Index 3*. Diese Änderungen würden in die Kategorie *Design for
Testability* fallen. Sie scheinen auch keine tiefen Eingriffe in die
Schnittstelle zu sein und sollten vielleicht in zukünftigen Versionen
berücksichtigt werden.
