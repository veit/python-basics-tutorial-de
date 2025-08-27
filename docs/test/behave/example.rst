Beispiel
========

#. Nachdem wir behave installiert haben, können wir im Verzeichnis
   :file:`features` eine Datei :file:`install.feature` mit folgendem Inhalt
   erstellen:

   .. code-block:: gherkin

      Feature: showing off behave

        Scenario: run a simple test
           Given we have behave installed
            When we implement a test
            Then behave will test it for us!

   .. seealso::
      `Features <https://behave.readthedocs.io/en/latest/tutorial/#features>`_

#. Anschließend erstellen wir im Verzeichnis :file:`features/steps` die Datei
   :file:`install.py`:

   .. code-block:: python

      from behave import *


      @given("we have behave installed")
      def step_impl(context):
          pass


      @when("we implement a test")
      def step_impl(context):
          assert True is not False


      @then("behave will test it for us")
      def step_impl(context):
          assert context.failed is False

   .. seealso::
      `Python Step Implementations
      <https://behave.readthedocs.io/en/latest/tutorial/#python-step-implementations>`_

#. ``behave`` aufrufen

   .. code-block:: console

      $ behave
      USING RUNNER: behave.runner:Runner
      Feature: showing off behave # features/install.feature:1

        Scenario: run a simple test        # features/install.feature:3
          Given we have behave installed   # features/steps/install.py:3 0.000s
          When we implement a test         # features/steps/install.py:7 0.000s
          Then behave will test it for us  # features/steps/install.py:11 0.000s

      1 feature passed, 0 failed, 0 skipped
      1 scenario passed, 0 failed, 0 skipped
      3 steps passed, 0 failed, 0 skipped
      Took 0min 0.000s
