Unified Modeling Language (UML)
===============================

Installation
------------

#. Installiert `plantuml <https://plantuml.com/starting>`_:

   * Installiert die Abhängigkeiten Java und Graphviz:

     .. tab:: Linux

        .. code-block:: console

           $ sudo apt install openjdk-11-jdk graphviz

   * Ladet die Datei `plantuml.jar
     <http://sourceforge.net/projects/plantuml/files/plantuml.jar/download>`_
     herunter.

#. Installiert `sphinxcontrib-plantuml
   <https://pypi.org/project/sphinxcontrib-plantuml/>`_:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ bin/python -m pip install sphinxcontrib-plantuml

   .. tab:: Windows

      .. code-block:: ps1con

         C:> Scripts\python -m pip install sphinxcontrib-plantuml

#. Konfiguriert Sphinx in der ``conf.py``-Datei:

   .. code-block:: python

    extensions = [
        ...,
        'sphinxcontrib.plantuml',
        ]

    plantuml = 'java -jar /PATH/TO/plantuml.jar'

   .. note::
      Auch in Windows werden in der Pfadangabe ``/`` angegeben.

.. toctree::
   :titlesonly:
   :hidden:

   sequence-diagram
   use-case-diagram
   activity-diagram
   class-diagram
