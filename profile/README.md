# Open Engineering Language Server

Engineering intelligence, right where you type.

![Open Engineering Language Server hero-banner.png](../assets/hero-banner.png)

Open Engineering Language Server provides IDE-native understanding of the Open Engineering Ecosystem.

It turns Open Engineering schemas, definitions, conventions, identifiers, registries, relationships, and compatibility rules into live engineering assistance: completion, validation, hover documentation, navigation, diagnostics, references, and contextual suggestions.

The goal is simple:

If Open Engineering knows it, your development environment should know it too.

⸻

Why Language Server?

Open Engineering describes an engineering landscape in machine-readable form.

Definitions describe what things are.
Conventions describe how they are expressed.
Schemas describe their structure.
Identifiers give them stable identities.
Registries tell us what exists.
The Open Engineering Map tells us how things relate.

That knowledge should not remain hidden in repositories and documentation.

It should be available while engineering.

Consider an Open Engineering document:

apiVersion: open-engineering.io/v1
kind: Pico
metadata:
  name: hello-pico
spec:

At spec:, an Open Engineering-aware editor should already know what can come next.

It might offer:

runtime
capabilities
state
interfaces
dependencies

When a property is selected, the editor should understand its expected type, documentation, constraints, relationships, and valid references.

Open Engineering Language Server makes that possible.

⸻

The Vision

We want the Open Engineering Ecosystem to behave, from an engineer’s perspective, like one enormous typed program.

A reference such as:

runtime: oee.runtime.celld.default

should not merely be a string.

It represents an engineering relationship.

The development environment should be able to answer:

* What is this?
* Is it valid?
* Where is it defined?
* What type of entity is expected here?
* What other entities could I use?
* Is this entity compatible with the surrounding configuration?
* Who references it?
* Is it deprecated?
* What documentation describes it?
* Can I navigate to its definition?

The Language Server turns Open Engineering metadata into that interactive engineering experience.

⸻

From Schema to Engineering Intelligence

At its simplest, Open Engineering Language Server provides schema-aware editing.

Schemas can describe:

* properties;
* types;
* required fields;
* enumerations;
* defaults;
* descriptions;
* examples;
* constraints;
* deprecated properties.

This already enables useful completion and validation.

But Open Engineering goes further.

A property might not merely be a string.

It might be a reference to another Open Engineering entity.

For example:

runtime:
  type: string
  description: Runtime responsible for executing this Pico.
  x-open-engineering:
    reference:
      kind: PicoRuntime

Standard JSON Schema tooling can ignore x-open-engineering.

Open Engineering Language Server can understand it.

The result is a schema that serves simultaneously as:

validation contract + documentation source + type definition + IDE intelligence source

⸻

Open Engineering References

Consider:

kind: Pico
spec:
  runtime:

The schema may declare that runtime expects a reference to a PicoRuntime.

Instead of suggesting arbitrary strings, the Language Server can query Open Engineering knowledge and offer known compatible entities.

For example:

oee.runtime.pico.default
oee.runtime.celld.default
oee.runtime.local.development

The same applies to artifacts, capabilities, interfaces, observers, definitions, components, services, packages, and other Open Engineering entities.

This gives the ecosystem something analogous to a distributed type system.

Pico
 │
 ├── runsOn ───────────────► PicoRuntime
 │
 ├── implements ───────────► Definition
 │
 ├── uses ─────────────────► Artifact
 │
 ├── exposes ──────────────► Interface
 │
 ├── requires ─────────────► Capability
 │
 └── observedBy ───────────► Observer

Open Engineering relationships become IDE-understandable relationships.

⸻

Architecture

Open Engineering Language Server sits between the engineering knowledge of the ecosystem and the tools in which engineers work.

Open Engineering Definitions
             │
Open Engineering Conventions
             │
Open Engineering Schemas
             │
             ▼
      Schema Intelligence
             │
             ├──────────────────────┐
             │                      │
             ▼                      ▼
Open Engineering Registries   Open Engineering Map
             │                      │
             └──────────┬───────────┘
                        │
                        ▼
              Compatibility Rules
                        │
                        ▼
        Open Engineering Language Server
                        │
                 Language Server
                   Protocol (LSP)
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
      Editors           IDEs         AI tooling

The Language Server Protocol keeps the core intelligence independent from any particular editor.

Editor-specific integrations should therefore remain deliberately thin.

⸻

Capabilities

The Language Server is intended to progressively support the standard capabilities developers expect from modern programming environments.

Completion

Suggest valid:

* properties;
* values;
* entity identifiers;
* definitions;
* artifacts;
* capabilities;
* runtimes;
* interfaces;
* relationships;
* registry entries.

For example:

dependencies:
  -

could offer compatible registered dependencies instead of arbitrary text.

Hover

Hovering over:

runtime: oee.runtime.celld.default

could show:

Celld Runtime
Kind: PicoRuntime
Identifier: oee.runtime.celld.default
Durable execution runtime compatible with
the Pico execution model.
Go to definition →

Diagnostics

Detect problems while editing:

Unknown Open Engineering identifier
oee.runtime.celld.defualt
                  ^^^^^^^

or:

Incompatible reference
Expected: PicoRuntime
Received: Artifact

Go to Definition

A referenced Open Engineering identifier should be navigable to the repository, metadata, schema, or definition that declares it.

Find References

Given an entity, discover where it is used throughout the Open Engineering landscape.

This turns ecosystem relationships into navigable engineering information.

Signature Help

Where Open Engineering structures behave like typed interfaces, show the expected properties and their meaning while the engineer is composing them.

Semantic Highlighting

Editors may visually distinguish:

* entity definitions;
* entity references;
* identifiers;
* capabilities;
* artifacts;
* relationships;
* deprecated elements.

Code Actions

Where safe, diagnostics should be actionable.

For example:

Unknown property: dependancies
Quick Fix:
→ Rename to "dependencies"

Rename

Where Open Engineering’s identifier and registry rules permit it, identifier changes should eventually support safe ecosystem-aware refactoring.

⸻

Context-Aware Completion

Schema completion is only the beginning.

Consider:

kind: Pico
spec:
  language: python
  runtime: celld
  artifact:

The Language Server has several pieces of context:

kind     = Pico
language = Python
runtime  = Celld

It should therefore not suggest every known artifact.

It should progressively narrow candidates to artifacts that satisfy the surrounding engineering constraints.

Schema
   +
Definitions
   +
Conventions
   +
Registry
   +
Open Engineering Map
   +
Compatibility Rules
        │
        ▼
Context-aware completion

The distinction is important.

A schema tells us:

What is structurally valid?

Open Engineering intelligence can additionally tell us:

What can actually work here?

⸻

Cross-Repository Intelligence

Open Engineering is intentionally distributed across many repositories and organizations.

That should not mean the engineer has to mentally reconstruct those relationships.

Repositories expose machine-readable metadata such as:

open-engineering-*/source/metadata.yaml

The Open Engineering Map can aggregate this information into a connected representation of the ecosystem.

The Language Server can consume that representation to provide cross-repository intelligence.

An engineer working in one repository can therefore discover compatible entities defined somewhere else without leaving the editor.

Repository A
    │
    │ metadata.yaml
    ▼
Open Engineering Map
    ▲
    │ metadata.yaml
    │
Repository B

To the engineer, these distributed repositories increasingly behave as one connected engineering environment.

⸻

Beyond YAML

Open Engineering intelligence should not be restricted to Open Engineering manifests.

An Open Engineering identifier can occur in source code.

Python

@pico("oee.pico.hello")
class HelloPico:
    ...

Rust

#[open_engineering::pico("oee.pico.hello")]
pub struct HelloPico;

TypeScript

const pico = oe.pico("oee.pico.hello");

The Language Server can eventually recognize these references and provide the same navigation and information regardless of the host language.

Open Engineering therefore complements rather than replaces existing language tooling.

A Python language server understands Python.

A Rust language server understands Rust.

Open Engineering Language Server understands the engineering meaning connecting them.

⸻

AI-Assisted Engineering

The same structured knowledge that helps humans can help engineering agents.

Instead of asking an AI system to infer the Open Engineering landscape from arbitrary repository contents, tools can consume explicit machine-readable information about:

* entity types;
* schemas;
* relationships;
* compatibility;
* identifiers;
* available components;
* constraints;
* documentation;
* provenance.

The Language Server can therefore become an important interface between the Open Engineering knowledge model and AI-assisted development environments.

AI assistance and deterministic engineering intelligence should complement each other.

                 Engineer
                    │
                    ▼
                  Editor
                    │
          ┌─────────┴─────────┐
          ▼                   ▼
    Language Server      AI Assistant
          │                   │
          └─────────┬─────────┘
                    ▼
          Open Engineering
             Knowledge

The schema remains authoritative.

AI helps interpret and compose.

⸻

Relationship to the Open Engineering Ecosystem

Open Engineering Language Server is not intended to become another source of truth.

It consumes existing Open Engineering sources of truth.

Definitions
     │
     ▼
Conventions
     │
     ▼
Schemas
     │
     ▼
Identifiers
     │
     ▼
Registries
     │
     ▼
Open Engineering Map
     │
     ▼
Language Server
     │
     ▼
Engineering Experience

This separation matters.

The Language Server should expose ecosystem knowledge, not redefine it.

⸻

Design Principles

Standards First

Prefer established standards wherever practical:

* Language Server Protocol;
* JSON Schema;
* YAML;
* JSON;
* URI-compatible identifiers;
* existing editor extension mechanisms.

Open Engineering extensions should augment standards rather than unnecessarily replace them.

Schema-Driven

Do not hard-code domain knowledge into editor plugins when that knowledge can be expressed through schemas or conventions.

Thin Editor Integrations

Keep Open Engineering intelligence in the Language Server.

Editor extensions should primarily provide transport, activation, configuration, and user experience integration.

Deterministic Before Generative

If a schema, registry, relationship, or compatibility rule can answer a question deterministically, use it.

AI assistance can operate on top of this foundation.

Explain Suggestions

Where possible, completion should not merely say what can be selected.

It should explain why.

For example:

oee.artifact.python.hello-pico
✓ Artifact
✓ Python compatible
✓ Pico compatible
✓ Celld compatible

Graceful Offline Operation

Local schemas and cached registry/map information should allow useful operation without requiring continuous network connectivity.

Open Ecosystem

The protocol and semantics should remain editor-independent and implementation-friendly so other Open Engineering-compatible tools can consume the same intelligence.

⸻

Implementation Roadmap

Phase 1 — Schema Intelligence

Establish the smallest useful implementation.

Support:

* YAML;
* JSON;
* Open Engineering schema discovery;
* validation;
* completion;
* hover documentation;
* diagnostics.

The objective is immediate value with minimal Open Engineering-specific machinery.

Phase 2 — Semantic Extensions

Define the x-open-engineering vocabulary for expressing concepts JSON Schema alone cannot adequately describe.

For example:

x-open-engineering:
  reference:
    kind: Artifact

Add:

* typed references;
* entity kinds;
* relationship semantics;
* identifier validation.

Phase 3 — Language Server

Implement the dedicated Open Engineering LSP server.

Initial LSP capabilities:

* completion;
* hover;
* diagnostics;
* go to definition;
* document symbols;
* references.

Phase 4 — Registry Intelligence

Connect the Language Server to Open Engineering Registries.

Completion can now include actual registered entities.

Phase 5 — Map Intelligence

Integrate the Open Engineering Map.

Enable:

* cross-repository navigation;
* incoming references;
* outgoing relationships;
* dependency discovery;
* ecosystem-aware hover information.

Phase 6 — Compatibility Intelligence

Evaluate Open Engineering compatibility rules while composing configurations.

Completion changes from:

everything matching the schema

to:

everything compatible with this context

Phase 7 — Source-Code Awareness

Recognize Open Engineering identifiers embedded in:

* Python;
* Rust;
* TypeScript/JavaScript;
* Kotlin;
* shell scripts;
* configuration;
* documentation.

Phase 8 — Agent Integration

Expose deterministic Open Engineering intelligence to AI engineering assistants and agents.

Agents should be able to ask questions such as:

What Artifact kinds are valid here?
Which PicoRuntime implementations satisfy this requirement?
Where is this identifier defined?
What depends on this component?
Why is this composition invalid?

The same answers presented to humans in the IDE become available to engineering automation.

⸻

Example Experience

Imagine building Hello, Pico!

The engineer writes:

apiVersion: open-engineering.io/v1
kind: Pico
metadata:
  name: hello-pico
spec:
  runtime:

The editor offers compatible Pico runtimes.

The engineer selects:

runtime: oee.runtime.celld.default

They continue:

artifact:

The editor now knows that this is:

* a Pico;
* using the selected runtime;
* expecting an Artifact.

It can suggest only artifacts satisfying those constraints.

Selecting one establishes another typed relationship.

Hovering shows documentation.

Command-clicking navigates to its definition.

Invalid combinations produce diagnostics.

References can be explored across repositories.

The engineer remains inside the development environment while Open Engineering provides the knowledge needed to assemble the system.

⸻

The Desired Developer Experience

Open Engineering should progressively make this interaction feel ordinary:

Engineer types
      │
      ▼
IDE understands context
      │
      ▼
Schema constrains structure
      │
      ▼
Registry supplies candidates
      │
      ▼
Map supplies relationships
      │
      ▼
Rules determine compatibility
      │
      ▼
Language Server explains
      │
      ▼
Engineer composes confidently

The complexity exists in the ecosystem.

It should not have to exist in the engineer’s head.

⸻

What Belongs Here?

The Open Engineering Language Server organization owns implementations concerned with bringing Open Engineering semantic intelligence into development environments.

Typical responsibilities include:

* the Open Engineering Language Server;
* LSP protocol implementation;
* schema discovery;
* semantic schema extensions;
* registry clients required for IDE intelligence;
* map clients required for IDE intelligence;
* completion providers;
* hover providers;
* diagnostic providers;
* navigation providers;
* compatibility-aware completion;
* editor integrations;
* reusable language-server libraries;
* testing fixtures and conformance suites.

Authoritative definitions, conventions, schemas, identifiers, registries, and ecosystem metadata remain owned by their respective Open Engineering domains.

⸻

What Does Not Belong Here?

This organization should not become the authoritative home for:

* Open Engineering Definitions;
* ecosystem-wide Conventions;
* canonical Registries;
* the Open Engineering Map;
* runtime implementations;
* Picos;
* Artifacts;
* general-purpose IDE functionality.

Those are upstream knowledge or downstream implementations.

This organization provides the bridge between them and the engineering environment.

⸻

Success

We know Open Engineering Language Server is succeeding when an engineer can encounter an unfamiliar Open Engineering configuration and confidently work with it without first searching through multiple repositories and documentation sites.

The editor should be able to answer:

What can I put here?

What does this mean?

Why is this invalid?

What is compatible with this?

Where is this defined?

What uses this?

And increasingly:

What should I compose next?

⸻

Open Engineering

Open Engineering Language Server is part of the broader Open Engineering Ecosystem — an effort to make engineering knowledge explicit, machine-readable, composable, navigable, and executable.

Definitions give engineering concepts meaning.

Conventions give them consistency.

Schemas give them structure.

Identifiers give them identity.

Registries make them discoverable.

The Map connects them.

The Language Server brings all of that knowledge to the engineer’s fingertips.

Define it. Connect it. Understand it. Compose it.
