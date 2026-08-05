Plugins
=======

Wir haben uns bisher auf die direkte Verwendung von :doc:`mock
<python3:library/unittest.mock>` konzentriert. Es gibt jedoch viele Plugins, die
beim Mocking helfen, wie :abbr:`z. B. (zum Beispiel)` `pytest-mock
<https://pypi.org/project/pytest-mock/>`_, das :abbr:`u. a. (unter anderem)`
eine ``mocker``-Fixture bereitstellt. Ein Vorteil ist, dass das Fixture nach
sich selbst aufräumt, so dass ihr keinen ``with``-Block verwenden müsst, wie wir
es in unseren Beispielen getan haben. Auch `mocker.spy
<https://pytest-mock.readthedocs.io/en/latest/usage.html#spy>`_ ist eine
praktische Vereinfachung, die auf ``wraps`` und ``autospec`` basiert.

Es gibt auch einige spezielle Mocking-Bibliotheken:

- Für das Mocking von Datenbankzugriffen eignen sich

  - `pytest-postgresql <https://pypi.org/project/pytest-postgresql/>`_
  - `pytest-mongo <https://pypi.org/project/pytest-mongo/>`_
  - `pytest-mysql <https://pypi.org/project/pytest-mysql/>`_
  - `pytest-dynamodb <https://pypi.org/project/pytest-dynamodb/>`_.

- Zum Testen von HTTP-Servern könnt ihr `pytest-httpserver
  <https://pypi.org/project/pytest_httpserver/>`_ verwenden.
- Zum Mocken von `requests <https://pypi.org/project/requests/>`_ könnt ihr
  `responses <https://pypi.org/project/responses/>`_ oder `betamax
  <https://pypi.org/project/betamax/>`_ verwenden.
- Weitere Tools für verschiedene Anforderungen sind

  - `pytest-rabbitmq <https://pypi.org/project/pytest-rabbitmq/>`_
  - `pytest-solr <https://pypi.org/project/pytest-solr/>`_
  - `pytest-elasticsearch <https://pypi.org/project/pytest-elasticsearch/>`_ und
    `pytest-redis <https://pypi.org/project/pytest-redis/>`_.
