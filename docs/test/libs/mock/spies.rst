Spies
=====

Im Gegensatz zu Mocks umhüllen :term:`Spies <Spy>` die Objekte und leiten
üblicherweise alle Methodenaufrufe an das Originalobjekt weiter. Trotz seines
Namens eignet sich :doc:`mock <python3:library/unittest.mock>` nicht nur für
Mocks: Über das Argument ``wraps`` seiner :class:`Mock
<python3:unittest.mock.Mock>`-Klasse können damit auch Spies erstellt werden.
Aufrufe werden dann an das umschlossene Objekt weitergeleitet und geben dessen
tatsächliche Ergebnisse zurück, :abbr:`z. B. (zum Beispiel)`:

.. code-block:: pycon

   >>> def test_workinngday():
   ...     spy = Mock(wraps=is_workingday, return_value=monday)
   ...     assert is_workingday()
   ...

``wraps`` stellt jedoch nur das Standardverhalten des Mocks bereit, wobei andere
Konfigurationen Vorrang haben. Wenn ihr ``return_value`` festlegt, wird das
umschlossene Objekt überhaupt nicht aufgerufen.

Wenn ihr hingegen :py:attr:`unittest.mock.Mock.side_effect` festlegt, wird
dieser anstelle des umschlossenen Objekts ausgeführt, es sei denn, er gibt
:py:data:`unittest.mock.DEFAULT` zurück; in diesem Fall wird der Aufruf
weitergeleitet.

So können wir :abbr:`z. B. (zum Beispiel)` für die Implementierung des
Löschbefehls überprüfen, ob die Tasks-CLI Fehlerbedingungen korrekt behandelt:

.. code-block:: python

    @app.command()
    def delete(task_id: int):
        """Remove task in db with given id."""
        with tasks_db() as db:
            try:
                db.delete_task(task_id)
            except tasks.InvalidTaskId:
                print(f"Error: Invalid task id {task_id}")

Um zu testen, wie die CLI mit einer Fehlerbedingung umgeht, können wir so tun,
als ob :func:`delete_task` eine Exception erzeugt, indem wir dem Mock-Objekt die
:class:`Exception` dem Attribut `side_effect
<https://docs.python.org/3/library/unittest.mock.html#unittest.mock.Mock.side_effect>`_
des Mock-Objekts zuweisen, etwa so:

.. code-block:: python

    def test_delete_invalid(mock_tasksdb, tasks_cli):
        mock_tasksdb.delete_task.side_effect = tasks.api.InvalidTaskId
        out = tasks_cli("delete 42")
        assert "Error: Invalid task id 42" in out
