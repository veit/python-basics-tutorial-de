Testgetriebene Entwicklung
==========================

:term:`Testgetriebene Entwicklung` (:term:`TDD`) zeichnet sich dadurch aus, dass
zunächst Tests für eine Funktion geschrieben werden, bevor die Funktion
implementiert wird. Genauer soll nur so viel implementiert werden, wie zum
Bestehen der Tests erforderlich ist.

Wiederholt diesen Prozess *„Erst testen,
dann implementieren“* so lange, bis die Funktion euren aktuellen Anforderungen
entspricht.

Diese Idee wurde Ende der 1990er Jahre von Kent Beck in seinem Buch
`„Test-Driven Development: By Example“
<https://archive.org/details/est-driven-development-by-example/test-driven-development-by-example/>`_
vorgestellt. Bekannt geworden sind die sich wiederholenden drei einfache
Schritte als  *„Red – Green – Refactor“*:

#. Schreiben von Tests für die nächste Funktion, die hinzugefügt werden soll.
#. Schreiben des Funktionscode, bis der Test bestanden ist.
#. Überarbeiten sowohl des neuen wie auch des alten Code, um ihn besser zu
   strukturieren.

TDD hat den Anspruch, dass der Prozess effizienter eine Anforderung
implementiert, die zudem für den jeweiligen Anwendungsfall gründlich getestet
ist. Es soll keine Zeit verschwendet werden, Optionen und Funktionen zu
implementieren, nur für den Fall, dass sie sich später als nützlich erweisen
könnten. Man soll exakt das bekommen, was man braucht, wenn man es braucht, und
nicht mehr.

Diese drei Schritte waren jedoch eine starke Verkürzung und so versuchte Kent
Beck Ende 2023 mit `Canon TDD <https://tidyfirst.substack.com/p/canon-tdd>`_
einige der Missverständnisse aufzulösen. Wie schon im `agilen Manifest
<https://agilemanifesto.org/>`_ werden Menschen und Interaktionen vor Prozesse
und Werkzeuge gestellt:

    „Wenn du einen anderen Arbeitsablauf als den folgenden verwendest und er für
    dich funktioniert, herzlichen Glückwunsch! Es ist zwar nicht das Canon-TDD,
    aber wen interessiert das schon? Es gibt keinen Sonderpreis dafür, diese
    Schritte genau zu befolgen.“

Erst dann stellt er die folgenden fünf Schritte vor:

#. Testliste

   Alle erwarteten Varianten des neuen Verhaltens werden aufgelistet: *„Das ist
   der Basisfall und was soll in diesem oder jenem Ausnahmefall passieren.“*
   Hier soll das Verhalten (engl.: Behavior) analysiert werden, **nicht**
   Software-Design oder -Implementierung.

   .. admonition:: Beispiel Mittelwertsberechnung
      :collapsible: closed

      Für eine Mittelwertsberechnung könnte die initiale Testliste
      folgendermaßen aussehen:

      * Der Basisfall ist, dass der Mittelwert aus einer Sequenz, einer Liste
        oder einem Iterator gebildet wird.
      * Es soll eine Zahl vom passenden Typ zurückgegeben werden, also
        :abbr:`ggf. (gegebenenfalls)` auch eine Ganzzahl.
      * Ist die Menge oder Sequenz leer, soll eine Fehlermeldung ausgegeben
        werden.
      * Sind ein oder mehrere  Elemente :doc:`../types/strings/index`, so soll
        versucht werden, diese in Zahlen vom passenden Typ umzuwandeln.
      * Gelingt die Umwandlung einzelner Elemente in Zahlen nicht, soll eine
        passende Fehlermeldung ausgegeben werden.

#. Schreibe einen Test

   Nur ein Test soll geschrieben werden mit *Setup*, *Invocation* und
   *Assertion*. Zwar werden beim Schreiben dieses Tests schon
   Design-Entscheidungen getroffen, aber es werden vor allem Entscheidungen zum
   Interface sein, nicht zur Implementierung selbst.

   .. admonition:: Beispiel Mittelwertsberechnung
      :collapsible: closed

      Der Basistest könnte so aussehen:

      .. code-block:: python

         @pytest.mark.xfail(
             strict=True, raises=AssertionError, reason="Not implemented yet"
         )
         def test_mean_base():
             ls = [1, 2, 3]
             tp = tuple(ls)
             st = set(ls)
             assert mean(ls) == mean(tp) == mean(st) == 2

      Wir haben lediglich festgelegt, dass die Funktion :func:`mean` heißen soll
      und als Parameter eine :doc:`../types/sequences-sets/lists`, ein
      :doc:`../types/sequences-sets/tuples` oder ein
      :doc:`../types/sequences-sets/sets` verarbeitet werden kann.

      Mit dem :doc:`Dekorator <../functions/decorators>`
      :func:`@pytest.mark.xfail` erwarten wir, dass dieser Test zunächst
      fehlschlägt.

      Als Nächstes schreiben wir eine minimale Version von :func:`mean`, bei der
      unser Test fehlschlagen sollte:

      .. code-block:: python

         def mean(se):
             pass

      .. code-block:: pytest

         $ uv run pytest -v test_mean.py
         ============================= test session starts ==============================
         ...

         test_mean.py::test_mean_base XFAIL (Not implemented yet)                 [100%]

         ============================== 1 xfailed in 0.08s ==============================

   Den Test **vor** der Implementierung zu schreiben hat folgende Vorteile:

   * Die Implementierung ist schneller, da wir mit dem Test bereits Code haben
     um die Implementierung aufzurufen.
   * Dies gewährleistet, dass der Test auch wirklich fehlschlägt.

     Bei Legacy-Code schreiben wir einen Tests für ein bereits vorhandenes
     Verhalten. Um zu gewährleisten, dass der Test auch fehlschlagen kann,
     löschen wir dann kurzzeitig die Implementierung. Schließlich holen wir die
     Implementierung wieder aus der Versionsverwaltung zurück.

   * Der Test bringt uns dazu, die Perspektive zu wechseln und den Fokus auf die
     Schnittstelle zum Aufruf des Codes zu legen.
   * Der Test zeigt uns an, wann der Implementierungsschritt abgeschlossen ist.

   Bei Legacy-Code wird das Schreiben von Tests schwieriger. Hier könnt ihr zwar
   auch einen Test für ein bereits vorhandenes Verhalten schreiben, aber mit dem
   Bestehen des Tests wisst ihr noch nicht, ob er auch fehlschlagen kann.
   Deswegen löschen wir die Implementierung kurzfristig um das Fehlschlagen des
   Tests gewährleisten zu können.

#. Den Test bestehen lassen

   Ändere den Code so, dass der Test (und alle vorherigen Tests) bestehen und
   wenn du feststellst, dass ein weiterer Test benötigt wird, füge ihn der
   Testliste hinzu.

   .. admonition:: Beispiel Mittelwertsberechnung
      :collapsible: closed

      Nun ändern wir unsere :func:`mean`-Funktion so ab, dass unser Test
      bestanden wird:

      .. code-block:: python

           def mean(se):
               return sum(se) / len(se)

      Wir entfernen nun den ``pytest.mark.xfail``-Dekorator, da wir erwarten,
      dass der Test nun bestanden wird:

      .. code-block:: pytest

         $ uv run pytest -v test_mean.py
         ============================= test session starts ==============================
         ...

         test_mean.py::test_mean_base PASSED                                      [100%]

         ============================== 1 passed in 0.15s ===============================

   .. warning::

      In der Vergangenheit wurden bei diesem Schritt öfter Fehler gemacht:

      * Assertions löschen, damit der Test bestanden wird
      * Von der Funktion berechnete Werte in die Testfunktion kopieren
      * Keine „zwei Hüte zu tragen“ und an dieser Stelle schon Refactoring
        machen wollen

#. Optional: Refactoring

   Jetzt ist die Zeit um Entscheidungen zum Implementierungsdesign treffen.

   .. warning::

      Auch bei diesem Schritt kommt es häufiger zu Fehlern:

      * Refactoring, das über dieses Verhalten hinausgeht
      * Zu frühe Abstraktion: So sind Duplikate sind lediglich Hinweise und
        kein zwingendes Gebot für ein Refactoring

#. Gehe zu 2. zurück, bis die Liste leer ist

   Teste und mplementiere so lange, bis das gewünschte Verhalten erreicht ist.
