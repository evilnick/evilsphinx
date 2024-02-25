# Markdown examples for Kroki

Well, more specifically, Myst examples. It is slightly more of a pain doing
this in Myst because everything has to be wrapped up in nonsense and indented
to satisfy sphinx, but it is possible and does work.

```{kroki}
    :type: mermaid
    
    %%{
    init: {
        "theme": "forest",
        "fontFamily": "ubuntu",
        "logLevel": "info"
    }
    }%%
    flowchart LR
    A[Start] --> B{Is the code finished?}
    B -->|Yes| C[Great!]
    B --->|No| G[Do another Pulse ]
    G --> B
    C --> D{Is it Friday?}
    D ----> |Yes| E[Release!!!]
    D -->|No| F[Run more tests]
    F --> D
```

## PlantUML C4

C4 is hierarchical model for representing computer systems (see the [C4 website](https://c4model.com/)).

Engineers go wild for tools like [Structurizr](https://structurizr.com/) which
turn expressions of models into nice hierarchical diagrams. These are diagrams
that were competed for **Canonical Kubernetes**. The original source is [this
Structurizr .dsl file](./c4.dsl). This could be used directly, but it is
slightly harder to control the output, plus like any 'generator' tool going
back to Netscape Composer, it tends to fill the file with useless cruft. The
examples presented here are the views extracted from the DSL in PlantUML format
(here's an [example file](./k8s/overview.puml)).

It renders like this:

```{kroki} ./k8s/overview.puml
```

The same master DSL file was used to generate all the required parts of the C4 model

```{kroki} ./k8s/k8sd-component.puml
```

```{kroki} ./k8s/k8s-context.puml
```