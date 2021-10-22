UI-Elemente und Interaktionen
=============================

.. rst:role:: guilabel

   Label, die als Teil einer interaktiven Benutzeroberfläche dargestellt werden,
   sollten mit :rst:role:`guilabel` gekennzeichnet werden. Jede in der
   Oberfläche verwendete Beschriftung sollte mit dieser Rolle gekennzeichnet
   werden, einschließlich Beschriftung von Schaltflächen, Fenstertiteln,
   Feldnamen, Menü- und Menüauswahlnamen und sogar Werte in Auswahllisten.

   Ein Tastenkürzel für die GUI-Beschriftung kann mit einem et-Zeichen (&)
   eingefügt werden; dieses führt in der Ausgabe zur Unterstreichung des
   Folgebuchstabens.

   :guilabel:`&Cancel` erzielt ihr :abbr:`z.B. (zum Beispiel)` mit folgender
   Auszeichnung:
   
   .. code-block:: rest

      :guilabel:`&Cancel`

   .. note::
      Wenn ihr ein et-Zeichen einfügen wollt, könnt ihr es einfach verdoppeln.

.. rst:role:: kbd

   Dies stellt eine Folge von Tasteneingaben dar. Welche Form die Tastenfolge
   hat, kann von plattform- oder anwendungsspezifischen Konventionen abhängen.
   Wenn es keine entsprechenden Konventionen gibt, sollten die Namen von
   Modifikatortasten ausgeschrieben werden, um die Zugänglichkeit zu verbessern.
   Auch sollte nicht auf eine bestimmte Tastaturbeschriftung referenziert
   werden.

   :kbd:`Ctrl-s` erzielt ihr :abbr:`z.B. (zum Beispiel)` mit folgender
   Auszeichnung:
   
   .. code-block:: rest

      :kbd:`Ctrl-s`

.. rst:role:: menuselection

   Eine Menüauswahl sollte mit der Rolle ``menuselection`` markiert werden.
   Diese wird verwendet, um eine komplette Sequenz zu markieren, einschließlich
   der Auswahl von Untermenüs und der Auswahl bestimmter Operationein oder
   beliebiger Untersequenzen. Die Namen der einzelnen Auswahlen sollten durch
   ``-->`` getrennt werden.

   :menuselection:`View --> Cell Toolba r--> Slideshow` erzielt ihr :abbr:`z.B.
   (zum Beispiel)` mit folgender Auszeichnung:

   .. code-block:: rest

      :menuselection:`View --> Cell Toolbar --> Slideshow`

   :rst:role:`menuselection` unterstützt genau wie :rst:role:`guilabel` auch
   Tastaturkürzel mit einem et-Zeichen (&).
