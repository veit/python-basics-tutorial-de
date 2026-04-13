Behavior-Driven Development
===========================

Dan North begann 2006, von Behavior-Driven Development (BDD) zu sprechen um
Missverständnisse über :doc:`testgetriebene Entwicklung <../tdd>` auszuräumen:

    *„Programmierer wollten wissen, wo sie anfangen sollten, was sie testen
    sollten und was nicht, wie viel sie auf einmal testen sollten, wie sie ihre
    Tests benennen sollten und wie sie verstehen konnten, warum ein Test
    fehlschlug.“*

– Dan North: `Introducing BDD <https://dannorth.net/blog/introducing-bdd/>`_

Er stellte fest, dass verständlicher war, wenn er sagte, sie sollten
Verhaltensweisen anstatt Funktionen testen. Genauer:

* Betrachtet Änderungen am System als Verhaltensänderungen. Die Entscheidung,
  welchen Test oder welche Testreihe als Nächstes geschrieben werden soll, fällt
  leichter, wenn ihr in Verhaltensweisen denkt. Was ist das nächstwichtigste
  Verhalten? Schreibt Tests dafür.
* Benennt eure Testfunktionen nach dem erwarteten Verhalten, auch wenn die
  Testnamen dadurch fast wie Sätze klingen. So wird klar, was falsch ist, wenn
  der Test fehlschlägt. Außerdem wird deutlich, welche Aspekte ihr im Test
  überprüfen solltet. Wenn ein Test aus einem Grund fehlschlagen kann, der nicht
  mit dem Namen des Tests übereinstimmt, sollte es sich um einen anderen Test
  handeln.
* Das Formulieren oder Umformulieren von Anforderungen als Given-When-Then (GWT)
  erleichtert das Schreiben von Tests.

Given-When-Then und Arrange-Act-Assert bedeuten eigentlich dasselbe, GWT passt
jedoch besser zur verhaltensorientierten Denkweise. Zudem lassen sich aus
Anforderungen, die als Given-When-Then geschrieben sind, sehr einfach
:term:`Akzeptanztests <Akzeptanztest>` erstellen.

Damit erleichtert BDD die agile Zusammenarbeit zwischen Entwicklung,
Qualitätssicherung und Product Owner an einem Softwareprojekt. In `Selling BDD
to the Business
<https://speakerdeck.com/tastapod/selling-bdd-to-the-business>`_ geht Dan North
genauer auf diesen Prozess ein:

    *„BDD ist eine agile Methodik der zweiten Generation, die von außen nach
    innen arbeitet, auf Pull-Prinzipien basiert, mehrere Interessengruppen
    einbezieht, mehrere Ebenen umfasst, einen hohen Automatisierungsgrad
    aufweist und agil ist. Sie beschreibt einen Zyklus von Interaktionen mit
    klar definierten Ergebnissen, der zur Lieferung funktionsfähiger, getesteter
    und relevanter Software führt.“*

Gherkin
-------

Die strukturierten Akzeptanzkriterien, die in normaler Sprache und nicht in Code
verfasst waren, wurden in der Beschreibungssprache Gherkin noch weiter
formalisiert, sodass sie automatisch geparst werden konnten.

.. code-block:: gherkin

   Scenario: Add a task to the database
     Given an empty database
      When a task with a summary is added
      Then the number of tasks should be 1
       and the queried task from the db should correspond to the added object.

Jedes Szenario soll ein Beispiel sein, das einen bestimmten Aspekt des
Verhaltens der Anwendung veranschaulichen soll.

In Python können sowohl `behave <https://behave.readthedocs.io/en/latest/>`_ als
auch `pytest-bdd <https://pypi.org/project/pytest-bdd/>`_ Gherkin
parsen.
