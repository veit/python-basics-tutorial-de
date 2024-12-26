UI-Elemente und Interaktionen
=============================

Für die Dokumentation des User Interfaces und dessen Interaktionen stellt Sphinx
drei verschiedene Rollen bereit: ``guilabel``, ``kbd`` und ``menuselection``:

.. list-table::
   :header-rows: 1

   * - Eingabe
     - Ausgabe
     - Anmerkungen
   * - .. code-block:: rest

          :guilabel:`Cancel`
     -  :guilabel:`Cancel`
     - Jede im User Interface verwendete Beschriftung kann mit dieser Rolle
       gekennzeichnet werden, einschließlich der Beschriftung von Schaltflächen,
       Fenstertiteln, Feld-, Menü- und Menüauswahl-Namen und Werten in
       Auswahllisten.
   * - .. code-block:: rest

          :guilabel:`&Cancel`
     -  :guilabel:`&Cancel`
     - Tastenkürzel für die GUI-Beschriftung können mit einem et-Zeichen (``&``)
       eingefügt werden; dieses führt in der Ausgabe zur Unterstreichung des
       Folgebuchstabens.

       .. note::
          Wenn ihr ein et-Zeichen einfügen wollt, könnt ihr es einfach
          verdoppeln.
   * - .. code-block:: rest

          :kbd:`Ctrl-s`
     -  :kbd:`Ctrl-s`
     - Dies stellt eine Folge von Tasteneingaben dar. Welche Form die
       Tastenfolge hat, kann von plattform- oder anwendungsspezifischen
       Konventionen abhängen. Dabei sollten die Namen von Modifikatortasten
       ausgeschrieben werden, um die Zugänglichkeit zu verbessern.
       Tastaturbeschriftung referenziert werden.
   * - .. code-block:: rest

          :menuselection:`File --> Save`
     - :menuselection:`File --> Save`
     - Eine Menüauswahl wird mit der Rolle ``menuselection`` gekennzeichnet.
       Diese markiert komplette Sequenz, einschließlich der Auswahl von
       Untermenüs, bestimmter Operationen oder beliebiger Untersequenzen. Die
       Namen der einzelnen Auswahlen werden durch ``-->`` getrennt.

       :rst:role:`menuselection` unterstützt genau wie :rst:role:`guilabel`
       Tastaturkürzel mit einem et-Zeichen (``&``).
