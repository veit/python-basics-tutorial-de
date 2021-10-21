Direktiven
==========

reStructuredText kann mit `Directives
<https://docutils.sourceforge.io/docs/ref/rst/directives.html>`_ erweitert
werden. Sphinx macht hiervon ausgiebig Gebrauch. Hier sind einige Beispiele:

Inhaltsverzeichnis
------------------

.. toctree::
   :maxdepth: 2

   start
   rest
   docstrings

.. code-block:: rest

   .. toctree::
      :maxdepth: 2

      start
      rest
      docstrings

Meta-Informationen
~~~~~~~~~~~~~~~~~~

.. sectionauthor:: Veit Schiele <veit@cusy.io>
.. codeauthor:: Veit Schiele <veit@cusy.io>

.. code-block:: rest

   .. sectionauthor:: Veit Schiele <veit@cusy.io>
   .. codeauthor:: Veit Schiele <veit@cusy.io>

Code-Block
~~~~~~~~~~

.. code-block:: python
   :emphasize-lines: 3,5

   def some_function():
       interesting = False
       print 'This line is highlighted.'
       print 'This one is not...'
       print '...but this one is.'

.. code-block:: rest

   .. code-block:: python
      :emphasize-lines: 3,5

      def some_function():
          interesting = False
          print 'This line is highlighted.'
          print 'This one is not...'
          print '...but this one is.'

Siehe auch
~~~~~~~~~~

.. seealso::
    `Sphinx Directives
    <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html>`_

.. code-block:: rest

   .. seealso::
       `Sphinx Directives
       <https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html>`_

Glossar
~~~~~~~

.. glossary::

   Environment
       Eine Struktur, in der Informationen über alle Dokumente unterhalb des
       Stammverzeichnisses gespeichert und für Querverweise verwendet werden. 
       Die Umgebung wird gespeichert, so dass nachfolgende Programmläufe nur
       neue und geänderte Dokumente lesen und parsen.

   Quellverzeichnis
       Das Verzeichnis, das einschließlich seiner Unterverzeichnisse alle
       Quelldateien für ein Sphinx-Projekt enthält.

.. code-block:: rest

   .. glossary::

      Environment
          Eine Struktur, in der Informationen über alle Dokumente unterhalb des
          Stammverzeichnisses gespeichert und für Querverweise verwendet werden. 
          Die Umgebung wird gespeichert, so dass nachfolgende Programmläufe nur
          neue und geänderte Dokumente lesen und parsen.

      Quellverzeichnis
          Das Verzeichnis, das einschließlich seiner Unterverzeichnisse alle
          Quelldateien für ein Sphinx-Projekt enthält.
