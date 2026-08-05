``assert_called``
=================

Bisher haben wir die Rückgabewerte einer Mocking-Methode verwendet, um
sicherzustellen, dass unser Anwendungscode mit den Rückgabewerten richtig
umgeht. Aber manchmal gibt es keinen nützlichen Rückgabewert, :abbr:`z. B. (zum
Beispiel)` bei :samp:`tasks add some tasks -o veit`. In diesen Fällen
können wir das Mock-Objekt mit :func:`assert_called_with` fragen, ob es korrekt
aufgerufen wurde. Nach dem Aufruf von :func:`tasks_cli("add some tasks -o
veit")` wird nicht die API verwendet, um zu prüfen, ob das Element in die
Datenbank gelangt ist, sondern ein Mock, um sicherzustellen, dass die CLI die
richtige API-Methode korrekt aufgerufen hat. Die Implementierung des Befehls
:func:`add` ruft schließlich :func:`db.add_task` mit einem ``Task``-Objekt auf:

.. _test_add_with_owner:

.. code-block:: python
   :emphasize-lines: 4

   def test_add_with_owner(mock_tasksdb, tasks_cli):
       tasks_cli("add some task -o veit")
       expected = tasks.Task("some task", owner="veit", state="todo")
       mock_tasksdb.add_task.assert_called_with(expected)

Wenn :func:`add_task` nicht aufgerufen wird oder mit dem falschen Typ oder dem
falschen Objektinhalt aufgerufen wird, schlägt der Test fehl. Wenn wir
:abbr:`z.B. (zum Beispiel)` in ``expected`` den String ``"Veit"`` groß
schreiben, aber nicht im CLI-Aufruf, erhalten wir folgende Ausgabe:

.. code-block:: pytest
   :emphasize-lines: 10-13, 16

   $ uv run pytest -s tests/cli/test_add.py::test_add_with_owner
   ============================= test session starts ==============================
   ...
   configfile: pyproject.toml
   plugins: cov-4.1.0, Faker-19.11.0
   collected 1 item

   tests/cli/test_add.py F
   ...
   >           raise AssertionError(_error_message()) from cause
   E           AssertionError: expected call not found.
   E           Expected: add_task(Task(summary='some task', owner='Veit', state='todo', id=None))
   E           Actual: add_task(Task(summary='some task', owner='veit', state='todo', id=None))
   ...
   =========================== short test summary info ============================
   FAILED tests/cli/test_add.py::test_add_with_owner - AssertionError: expected call not found.
   ============================== 1 failed in 0.08s ===============================

.. seealso::
   Es gibt eine ganze Reihe von Varianten von :func:`assert_called`. Eine
   vollständige Liste und Beschreibung erhaltet ihr in
   :py:meth:`unittest.mock.Mock.assert_called`.

   Wenn die einzige Möglichkeit zum Testen darin besteht, den korrekten Aufruf
   sicherzustellen, erfüllen die verschiedenen :meth:`assert_called*`-Methoden
   ihren Zweck.
