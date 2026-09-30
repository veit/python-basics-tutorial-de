``autospec``
============

Mock-Objekte sind in der Regel als Objekte gedacht, die anstelle der echten
Implementierung verwendet werden. Standardmäßig werden sie jedoch jeden Zugriff
akzeptieren. Wenn das echte Objekt beispielsweise :func:`.start(index)` zulässt,
sollen unsere Mock-Objekte ebenfalls :func:`.start(index)` zulassen. Dabei gibt
es jedoch ein Problem. Mock-Objekte sind standardmäßig zu flexibel: sie
würden auch :func:`stort` oder andere falsch geschriebene, umbenannte oder
gelöschte Methoden oder Parameter akzeptieren. Dabei kann es im Laufe der Zeit
zum :abbr:`sog. (sogenannten)` Mock-Drift kommen, wenn sich die Schnittstelle,
die ihr nachbildet, ändert, euer Mock in eurem Testcode jedoch nicht. Diese Form
des Mock-Drifts kann durch das Hinzufügen von ``autospec=True`` zum Mock während
der Erstellung gelöst werden:

.. code-block:: python
   :emphasize-lines: 3

    @pytest.fixture()
    def mock_tasksdb():
        with mock.patch.object(tasks, "TasksDB", autospec=True) as MockTasksDB:
            yield MockTasksDB.return_value

Üblicherweise wird dieser Schutz mit ``autospec`` immer eingebaut. Die einzige
mir bekannte Ausnahme ist, wenn die Klasse oder das Objekt, das gemockt wird,
dynamische Methoden hat oder wenn Attribute zur Laufzeit hinzugefügt werden.

.. seealso::
   Die Python-Dokumentation hat einen großen Abschnitt über ``autospec``:
   :ref:`python3:auto-speccing`.
