# Nodus — Overview

> *Working title. Final name is an open question — see [[Open Questions]].*

---

## What it is

Nodus is a minimal, dark web application for visually organizing information as nodes on a spatial canvas, connected by meaningful, labeled relationships.

It is not a productivity tool. It is not a mind map. It is not a dashboard.

It is closer to **a quiet spatial environment where you define how pieces of information relate to each other** — and where those relationships are themselves objects with meaning, not just lines.

---

## Core Concept

Two things exist in the system:

**Nodes** — individual pieces of information. An idea, a person, a project, a concept, a task, a source, a note. Anything.

**Relationships** — named connections between nodes. Not visual links. Data objects. A relationship has a label that describes what it means: *depends on*, *inspired by*, *contradicts*, *belongs to*, *precedes*.

> The connection between two things is as meaningful as the things themselves.

Over time, the canvas becomes a personal map of how your ideas and projects relate to each other.

---

## What Makes This Different

| Traditional mind map                | Nodus                             |
| ----------------------------------- | --------------------------------- |
| Tree structure, hierarchical        | Freeform spatial placement        |
| Connections are visual decoration   | Connections are data with meaning |
| Focused on outlines / brainstorming | Focused on knowledge organization |
| Dense, cluttered interfaces         | Maximum negative space            |
| Always-visible toolbars             | Contextual, minimal UI            |
| Optimized for presentations         | Optimized for personal clarity    |

---

## The Intended Experience

The user opens a near-empty dark canvas.

They double-click to create a node. They type. They press Enter.

They create another node. They drag from one to the other to define a relationship. They type what that relationship means.

As more nodes and connections accumulate, the canvas becomes a visual map of their thinking — structured by the relationships they defined, not by a hierarchy someone else imposed.

The interface gets out of the way. Empty space is part of the design. When nothing is being interacted with, the canvas is nearly silent.

---

## Non-Goals

The application should never become:

- A traditional project management tool
- A collaborative whiteboard (Miro / FigJam direction)
- A feature-heavy SaaS product
- A complicated dashboard with panels and sidebars
- Obsidian (text-first, graph is secondary)
- Notion (database-first)

The focus is: **spatial + relational + minimal + personal**.

---

## Stack (initial version)

Plain HTML, plain CSS, plain JavaScript.

No frontend framework.

The initial version prioritizes:
- Correctness
- Understandability
- Good interaction quality

The architecture should not prevent future evolution (framework migration, cloud persistence, mobile) but should not prematurely optimize for it either.
