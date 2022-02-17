``unittest2``
=============

`unittest2 <https://pypi.org/project/unittest2/>`_ ist ein Backport von
:mod:`unittest`, mit verbesserter API und besseren *Assertions* als in früheren
Python-Versionen.

Beispiel
--------

Möglicherweise wollt ihr das Modul unter dem Namen ``unittest`` importieren um
die Portierung von Code auf neuere Versionen des Moduls in Zukunft zu
vereinfachen:

.. code-block:: python

    import unittest2 as unittest

    class MyTest(unittest.TestCase):
        ...

Auf diese Weise könnt ihr, wenn ihr zu einer neueren Python-Version wechselt und
das Modul ``unittest2`` nicht mehr benötigt, einfach den Import in eurem
Testmodul ändern, ohne dass ihr weiteren Code ändern müsst.

Installation
------------

   .. tab:: Linux/MacOS

      .. code-block:: console

         $ bin/python -m pip install unittest2

   .. tab:: Windows

      .. code-block:: ps1con

         C:> Scripts\python -m pip install unittest2
