# Nodus — Data Model

---

## Fundamental Principle

The DOM is not the source of truth. State is.

All nodes and relationships exist as data. The renderer reads that data and produces the visual output. Interactions update the data. The renderer reflects the update.

---

## Node

A node is the minimal unit of information.

```js
{
  id: "node_a1b2",      // unique ID, generated on creation
  title: "Research",    // editable, the primary label shown on canvas
  description: "",      // optional, shown in expanded/selected state
  x: 400,              // world-space X position
  y: 300,              // world-space Y position
}
```

### Future node properties (not MVP)

```js
{
  type: "idea",          // optional node type for subtle visual distinction
  collapsed: false,      // for future subgraph collapsing
  createdAt: 1700000000, // timestamp
  metadata: {}           // extensible key-value store
}
```

### Key principle

Node position (`x`, `y`) is a **world-space coordinate**, not a screen coordinate. The camera transforms world space into screen space. Never store screen-space pixel positions in node data.

---

## Relationship

A relationship is a named, directional connection between two nodes.

Relationships are first-class data objects — not visual side effects of connecting nodes.

```js
{
  id: "rel_c3d4",         // unique ID
  source: "node_a1b2",   // ID of the source node
  target: "node_e5f6",   // ID of the target node
  label: "inspired by",  // human-readable relationship label
}
```

### Future relationship properties (not MVP)

```js
{
  direction: "forward",  // "forward" | "backward" | "bidirectional"
  type: "semantic",      // category hint for rendering
  weight: 1,             // future: visual prominence
  metadata: {}
}
```

### Built-in label suggestions

These are defaults the user can choose from or ignore. They can also type anything custom.

- related to
- depends on
- inspired by
- created by
- belongs to
- follows
- precedes
- supports
- contradicts
- prerequisite for

---

## Graph State (data layer)

This is the **persistent, serializable** part of the application state.

```js
{
  nodes: [
    { id: "node_a1b2", title: "Research", description: "", x: 400, y: 300 },
    { id: "node_e5f6", title: "Article",  description: "", x: 720, y: 300 }
  ],
  relations: [
    { id: "rel_c3d4", source: "node_a1b2", target: "node_e5f6", label: "inspired by" }
  ]
}
```

This is exactly what gets serialized to `localStorage` (and eventually cloud persistence).

---

## Camera State

Camera state controls which part of the world is currently visible.

```js
{
  x: 0,    // pan offset X (world units)
  y: 0,    // pan offset Y (world units)
  zoom: 1  // scale factor
}
```

Camera state **is not part of graph data** — it is view-layer state.

Whether camera should persist across sessions (so the user returns to the same view) is an open question.

### World space vs screen space

```
WORLD SPACE
  Node positions live here.
  (400, 300) is an absolute world coordinate.
  It does not change when you pan or zoom.

CAMERA
  Defines where the viewport is positioned and scaled in world space.

SCREEN SPACE
  What the user sees on screen.
  Derived from: world position → apply camera transform → screen pixel.
```

The canvas renderer applies the camera transform. Node positions are never mutated by panning or zooming.

---

## UI / Interaction State

This is **ephemeral state** — it exists at runtime but is not persisted.

```js
{
  selectedNodeId: null,         // currently selected node, or null
  selectedRelationId: null,     // currently selected relationship, or null
  draggingNodeId: null,         // node currently being dragged
  creatingRelation: {           // active relationship being drawn
    active: false,
    sourceNodeId: null,
    currentX: 0,
    currentY: 0
  },
  editingNodeId: null,          // node currently in title-edit mode
  editingRelationId: null,      // relation label currently being edited
}
```

Keep this completely separate from graph data. Mixing ephemeral interaction state into the graph would pollute persistence and make serialization fragile.

---

## Full Application State Shape

```js
{
  // --- PERSISTENT ---
  graph: {
    nodes: [],
    relations: []
  },

  // --- PERSISTENT (maybe) ---
  camera: {
    x: 0,
    y: 0,
    zoom: 1
  },

  // --- EPHEMERAL ---
  ui: {
    selectedNodeId: null,
    selectedRelationId: null,
    draggingNodeId: null,
    editingNodeId: null,
    editingRelationId: null,
    creatingRelation: {
      active: false,
      sourceNodeId: null,
      currentX: 0,
      currentY: 0
    }
  }
}
```

---

## Persistence / Serialization

The serialized format written to `localStorage` is just the graph + camera:

```js
{
  version: 1,
  graph: {
    nodes: [...],
    relations: [...]
  },
  camera: {
    x: 0,
    y: 0,
    zoom: 1
  }
}
```

A `version` field makes future migration possible if the schema changes.

Persistence logic should be isolated in its own module so it can later be swapped for IndexedDB, a backend API, or cloud sync without touching the rest of the application.
