Kroki examples for .rst docs
============================


Kroki is a hosted diagram generation service which supports a huge variety of formats

Mermaid
-------

.. kroki::
   :type: mermaid
   :align: center

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

Mermaid can itself be used for different types of diagram.

.. kroki::
   :type: mermaid

   gantt
    title A Gantt Diagram
    dateFormat  YYYY-MM-DD
    section Section
    A task           :a1, 2024-01-01, 30d
    Another task     :after a1, 20d
    section Another
    Task in sec      :2024-01-12, 12d
    another task     :24d

.. kroki::
    :type: mermaid
    
    %%{
    init: {
        "theme": "forest",
        "fontFamily": "ubuntu",
        "logLevel": "info"
    }
    }%%
    flowchart TD
    A[Start] --> B{Is the code finished?}
    B -->|Yes| C[Great!]
    B --->|No| G[Do another Pulse ]
    G --> B
    C --> D{Is it Friday?}
    D ----> |Yes| E[Release!!!]
    D -->|No| F[Run more tests]
    F --> D

NWDiag
------

.. kroki::
    :type: nwdiag

    nwdiag {
        network dmz {
            address = "210.x.x.x/24"

            web01 [address = "210.x.x.1"];
            app01 [address = "210.x.x.8"];
        }
        network internal {
            address = "172.x.x.x/24";

            web01 [address = "172.x.x.1"];
            app01 [address = "172.x.x.2"];
            k8s01;
            k8s02;
            COS;
        }
        }

PlantUML
--------

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

PlantUML can be themed

.. kroki::
    :type: plantuml
    :align: center
    :class: "mermaid"

    @startuml
    !theme amiga from https://raw.githubusercontent.com/plantuml/plantuml/master/themes
    a -> b
    b -> c
    @enduml

.. kroki:: ./mm.puml png

.. kroki::
    :type: plantuml

    @startuml
    !include C4_Container.puml

    LAYOUT_TOP_DOWN()
    LAYOUT_WITH_LEGEND()

    title Container diagram for Internet Banking System

    Person(customer, Customer, "A customer of the bank, with personal bank accounts")

    System_Boundary(c1, "Internet Banking") {
        Container(web_app, "Web Application", "Java, Spring MVC", "Delivers the static content and the Internet banking SPA")
        Container(spa, "Single-Page App", "JavaScript, Angular", "Provides all the Internet banking functionality to cutomers via their web browser")
        Container(mobile_app, "Mobile App", "C#, Xamarin", "Provides a limited subset of the Internet banking functionality to customers via their mobile device")
        ContainerDb(database, "Database", "SQL Database", "Stores user registraion information, hased auth credentials, access logs, etc.")
        Container(backend_api, "API Application", "Java, Docker Container", "Provides Internet banking functionality via API")
    }

    System_Ext(email_system, "E-Mail System", "The internal Microsoft Exchange system")
    System_Ext(banking_system, "Mainframe Banking System", "Stores all of the core banking information about customers, accounts, transactions, etc.")

    Rel(customer, web_app, "Uses", "HTTPS")
    Rel(customer, spa, "Uses", "HTTPS")
    Rel(customer, mobile_app, "Uses")

    Rel_Neighbor(web_app, spa, "Delivers")
    Rel(spa, backend_api, "Uses", "async, JSON/HTTPS")
    Rel(mobile_app, backend_api, "Uses", "async, JSON/HTTPS")
    Rel_Back_Neighbor(database, backend_api, "Reads from and writes to", "sync, JDBC")

    Rel_Back(customer, email_system, "Sends e-mails to")
    Rel_Back(email_system, backend_api, "Sends e-mails using", "sync, SMTP")
    Rel_Neighbor(backend_api, banking_system, "Uses", "sync/async, XML/HTTPS")
    @enduml

bytefield
---------

.. kroki::
    :type: bytefield

    (defattrs :bg-green {:fill "#a0ffa0"})
    (defattrs :bg-yellow {:fill "#ffffa0"})
    (defattrs :bg-pink {:fill "#ffb0a0"})
    (defattrs :bg-cyan {:fill "#a0fafa"})
    (defattrs :bg-purple {:fill "#e4b5f7"})

    (defn draw-group-label-header
    "Creates a small borderless box used to draw the textual label headers
    used below the byte labels for remotedb message diagrams.
    Arguments are the number of columns to span and the text of the
    label."
    [span label]
    (draw-box (text label [:math {:font-size 12}]) {:span    span
                                                    :borders #{}
                                                    :height  14}))

    (defn draw-remotedb-header
    "Generates the byte and field labels and standard header fields of a
    request or response message for the remotedb database server with
    the specified kind and args values."
    [kind args]
    (draw-column-headers)
    (draw-group-label-header 5 "start")
    (draw-group-label-header 5 "TxID")
    (draw-group-label-header 3 "type")
    (draw-group-label-header 2 "args")
    (draw-group-label-header 1 "tags")
    (next-row 18)

    (draw-box 0x11 :bg-green)
    (draw-box 0x872349ae [{:span 4} :bg-green])
    (draw-box 0x11 :bg-yellow)
    (draw-box (text "TxID" :math) [{:span 4} :bg-yellow])
    (draw-box 0x10 :bg-pink)
    (draw-box (hex-text kind 4 :bold) [{:span 2} :bg-pink])
    (draw-box 0x0f :bg-cyan)
    (draw-box (hex-text args 2 :bold) :bg-cyan)
    (draw-box 0x14 :bg-purple)

    (draw-box (text "0000000c" :hex [[:plain {:font-weight "light" :font-size 16}] " (12)"])
                [{:span 4} :bg-purple])
    (draw-box (hex-text 6 2 :bold) [:box-first :bg-purple])
    (doseq [val [6 6 3 6 6 6 6 3]]
        (draw-box (hex-text val 2 :bold) [:box-related :bg-purple]))
    (doseq [val [0 0]]
        (draw-box val [:box-related :bg-purple]))
    (draw-box 0 [:box-last :bg-purple]))

    (draw-remotedb-header 0x4702 9)

    (draw-box 0x11)
    (draw-box 0x2104 {:span 4})
    (draw-box 0x11)
    (draw-box 0 {:span 4})
    (draw-box 0x11)
    (draw-box (text "length" [:math] [:sub 1]) {:span 4})
    (draw-box 0x14)

    (draw-box (text "length" [:math] [:sub 1]) {:span 4})
    (draw-gap "Cue and loop point bytes")

    (draw-box nil :box-below)
    (draw-box 0x11)
    (draw-box 0x36 {:span 4})
    (draw-box 0x11)
    (draw-box (text "num" [:math] [:sub "hot"]) {:span 4})
    (draw-box 0x11)
    (draw-box (text "num" [:math] [:sub "cue"]) {:span 4})

    (draw-box 0x11)
    (draw-box (text "length" [:math] [:sub 2]) {:span 4})
    (draw-box 0x14)
    (draw-box (text "length" [:math] [:sub 2]) {:span 4})
    (draw-gap "Unknown bytes" {:min-label-columns 6})
    (draw-bottom)

Vega
----

.. kroki:: 
    :type: vega
    :filename: ./vega-example2.vg


Excalidraw
----------

.. kroki:: evil.excalidraw svg