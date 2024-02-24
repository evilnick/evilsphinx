Kroki examples for .rst docs
============================


Kroki is a hosted diagram generation service which supports a huge variety of formats

Mermaid -

.. kroki::
   :type: mermaid

   sequenceDiagram
    participant Alice
    participant Bob
    Alice->John: Hello John, how are you?
    loop Healthcheck
        John->John: Fight against hypochondria
    end
    Note right of John: Rational thoughts prevail...
    John-->Alice: Great!
    John->Bob: How about you?
    Bob-->John: Jolly good!

PlanUML

.. kroki::
   :type: plantuml
   
    @startuml
    left to right direction
    skinparam packageStyle rectangle
    skinparam monochrome false
    actor customer
    actor clerk
    rectangle checkout {
      customer -- (checkout)
      (checkout) .> (payment) : include
      (help) .> (checkout) : extends
      (checkout) -- clerk
    }
    @enduml

.. kroki::
    :type: plantuml

    @startuml
    !theme amiga from https://raw.githubusercontent.com/plantuml/plantuml/master/themes
    a -> b
    b -> c
    @enduml

