Weitere Direktiven
==================

reStructuredText kann mit `Directives
<https://docutils.sourceforge.io/docs/ref/rst/directives.html>`_ erweitert
werden. Sphinx macht hiervon ausgiebig Gebrauch. Hier sind einige Beispiele:

Inhaltsverzeichnis
------------------

.. toctree::
   :maxdepth: 2

   start
   docstrings

.. code-block:: rest

   .. toctree::
      :maxdepth: 2

      start
      docstrings

Meta-Informationen
~~~~~~~~~~~~~~~~~~

.. sectionauthor:: Veit Schiele <veit@cusy.io>
.. codeauthor:: Veit Schiele <veit@cusy.io>

.. code-block:: rest

   .. sectionauthor:: Veit Schiele <veit@cusy.io>
   .. codeauthor:: Veit Schiele <veit@cusy.io>

.. note::
   Standardmäßig wird diese Angabe nicht in der Ausgabe berücksichtigt, bis ihr
   die Konfiguration für ``show_authors`` auf ``True`` setzt.

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
