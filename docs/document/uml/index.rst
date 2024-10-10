Unified Modeling Language (UML)
===============================

Installation
------------

#. Installiert `plantuml <https://plantuml.com/starting>`_:

   .. tab:: Linux

       .. code-block:: console

         $ sudo apt install plantuml

   .. tab:: macOS

      .. code-block:: console

         $ brew install plantuml

#. Installiert `sphinxcontrib-plantuml
   <https://pypi.org/project/sphinxcontrib-plantuml/>`_:

   .. tab:: Linux/macOS

      .. code-block:: console

         $ python -m pip install sphinxcontrib-plantuml

   .. tab:: Windows

      .. code-block:: ps1con

         C:> python -m pip install sphinxcontrib-plantuml

#. Konfiguriert Sphinx in der ``conf.py``-Datei:

   .. code-block:: python

      extensions = [..., "sphinxcontrib.plantuml"]

      plantuml = "/PATH/TO/PLANTUML"

   .. note::
      Auch in Windows werden in der Pfadangabe ``/`` angegeben.

.. toctree::
   :titlesonly:
   :hidden:

   sequence-diagram
   use-case-diagram
   activity-diagram
   class-diagram
