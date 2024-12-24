Objektorientierung
==================

Python bietet volle Unterstützung für `Objektorientierte Programmierung
<https://de.wikipedia.org/wiki/Objektorientierte_Programmierung>`_ (:abbr:`OOP
(Objektorientierte Programmierung)`).

.. graphviz::
   :layout: neato

    graph oop {

    graph [fontname = "Calibri", fontsize="16", overlap=false];
        node [fontname = "Calibri", fontsize="16", style="bold", penwidth="5px"];
        edge [fontname = "Calibri", fontsize="16", style="bold", penwidth="5px"];
        tooltip="Objektorientierte Programmierung";

        oop [
            label="Objektorientierte\nProgrammierung\n(OOP)",
            color="#FF66B3"]
        objects [
            label="Objekte",
            color="#BF80FF"]
        polymorphism [
            label="Polymorphismus",
            color="#9999FF"]
        classes [
            label="Klassen",
            color="#00FF80"]
        inheritance [
            label="Vererbung",
            color="#4da6ff"]
        encapsulation [
            label="Kapselung",
            color="#00FFFF"]
        oop -- objects [color="#FF66B3;0.5:#BF80FF"]
        oop -- polymorphism [color="#FF66B3;0.5:#9999FF"]
        oop -- classes [color="#FF66B3;0.5:#00FF80"]
        oop -- inheritance [color="#FF66B3;0.5:#4da6ff"]
        oop -- encapsulation [color="#FF66B3;0.5:#00FFFF"]
    }

.. toctree::
   :titlesonly:
   :hidden:

   classes
   variables
   methods
   inheritance
   coherent
   private
   property
   namespaces
   types
   dataclasses
   design/index
