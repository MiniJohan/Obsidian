# Nodus — Interaction Specification

---

## Core Philosophy

> Direct manipulation over forms and menus.

Every interaction should feel like touching an object directly:

- Create: double-click the canvas, type, press Enter
- Move: click and drag a node
- Connect: drag from one node to another
- Edit: click to select, then interact
- Delete: select, press Backspace

No modals. No multi-step wizards. No intermediate confirmation dialogs for basic operations.

---

## Creating Nodes

**Primary interaction:** Double-click empty canvas space

1. A new node appears at the click position
2. It immediately enters **editing mode** — the title field is active
3. The user types the title
4. `Enter` confirms — node exits editing mode, stays selected
5. `Escape` cancels — if no title was typed, the node is removed

The node should appear instantly. No animation delay before the user can type.

**Secondary interaction (keyboard):** `N` key

- Creates a node at the center of the current viewport
- Immediately enters editing mode

Avoid placing the create button in a permanent toolbar. The empty canvas hint ("double-click to create") is the only persistent affordance.

---

## Editing Nodes

**Node title:**
- Click a selected node (i.e., click it when it is already selected) → enters title editing mode
- Or press `Enter` while a node is selected
- Inline editing — the title text becomes an editable field in place
- `Enter` saves, `Escape` cancels

**Node description:**
- Only visible when the node is selected
- Click the description area (or a subtle "add note" affordance) to enter editing mode
- Saves on blur or `Enter`

---

## Moving Nodes

- Click and drag a node (from anywhere within its bounds) to move it
- Movement updates `x, y` in state in real time
- Relationships rerender in real time as the node moves
- The cursor should be `grab` → `grabbing` during drag
- Dragging should begin immediately — no delay, no drag threshold (or a minimal 4–5px threshold to avoid accidental drags on click)

Dragging a node does not select a different node. If the node was not selected before dragging, dragging it implicitly selects it.

---

## Selecting Nodes

- Click a node → selects it, deselects everything else
- Click empty canvas → deselects everything
- Selection visually distinguishes the node (accent border, raised background)
- Connected relationships may increase in visibility when a node is selected
- Non-connected nodes may dim slightly (subtle, not harsh)

Only one node or one relationship is selected at a time in MVP.

---

## Deleting Nodes

- Select a node → press `Backspace` or `Delete`
- The node is removed
- All relationships connected to that node are also removed (cascade delete)
- No confirmation dialog for deletion in MVP — but undo (`Ctrl/Cmd+Z`) should work

---

## Creating Relationships

The relationship creation gesture is the most important interaction in the application. It must feel natural and direct.

**Interaction flow:**

1. Hover a node → a subtle **connection affordance** appears (small dot or handle, colored `--accent-dim`)
2. Click and drag from that affordance
3. A line follows the cursor (a "ghost" relationship being drawn)
4. Hover over a target node → it highlights to indicate it can receive the connection
5. Release over a target node → relationship is created
6. The user is immediately prompted to **label the relationship** (see below)
7. Release over empty space → cancels (no relationship created)

**Relationship label entry:**

- After dropping onto a target node, the relationship is created
- A small inline input appears centered on the new relationship line
- The user types the label: "inspired by", "depends on", etc.
- `Enter` or blur confirms
- `Escape` creates the relationship without a label (which is valid — unlabeled relationships can be labeled later)

The ghost line during creation should be visually distinct from real relationships (dashed, dimmer).

---

## Editing Relationships

- Click a relationship line → selects it
- When selected, the label becomes visible and editable inline
- `Enter` or blur confirms
- `Escape` cancels

From a selected relationship:
- Press `Backspace` / `Delete` → deletes it
- Label is always editable via click

---

## Deleting Relationships

- Select a relationship → press `Backspace` or `Delete`
- No confirmation dialog

---

## Panning the Canvas

**Mouse:** Click and drag empty canvas space (no node, no relationship)

**Trackpad:** Two-finger drag

**Middle mouse button:** Click and drag (traditional)

Panning updates the camera state (`x`, `y`). Node positions in world space are not modified.

The canvas cursor should become `grab` when hovering empty space, `grabbing` during pan.

---

## Zooming the Canvas

**Mouse wheel:** Scroll up = zoom in, scroll down = zoom out

**Trackpad:** Pinch gesture

**Keyboard:** `+` / `-`

Zoom should be centered on the cursor position, not the canvas center. This is the expected behavior and is critical for a natural feel.

Zoom is bounded — very small or very large values should be clamped.

Suggested zoom range: `0.1` to `3.0`, with `1.0` as default.

---

## Canvas Reset

| Key | Action |
|---|---|
| `0` | Fit entire graph into view (calculate bounding box, zoom to fit with padding) |
| `1` | Zoom to selected node (center viewport on it at zoom ~1.0) |

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `N` | New node at viewport center |
| `Enter` | Edit selected node title |
| `Backspace` / `Delete` | Delete selected node or relationship |
| `Escape` | Deselect / cancel current action / exit editing |
| `0` | Fit graph to viewport |
| `1` | Zoom to selected |
| `+` / `=` | Zoom in |
| `-` | Zoom out |
| `Ctrl/Cmd + Z` | Undo |
| `Ctrl/Cmd + Shift + Z` | Redo |
| `Space` | Future: command palette |
| `F` | Future: search |
| `R` | Future: enter relationship creation mode |

---

## Contextual Controls

Controls appear when relevant, not permanently.

| State | What appears |
|---|---|
| Empty canvas | "Double-click anywhere to create" hint (fades out once a node exists) |
| Hovering node | Connection affordance dot |
| Node selected | Subtle delete control, edit affordance |
| Relationship selected | Label input, delete affordance |
| Mid-relationship-creation | Ghost line, target highlight on hover |

Controls should disappear cleanly when their context ends.

---

## Undo / Redo

Every state-modifying action should be undoable.

Actions that should be undoable:
- Create node
- Delete node
- Move node (on drag end, not on every drag frame)
- Edit node title
- Create relationship
- Delete relationship
- Edit relationship label

The undo stack does not need to be infinite in MVP, but it should cover reasonable use. A stack depth of 50 is a reasonable starting point.

---

## Touch / Mobile

Not a priority for MVP. The app is desktop-first.

However, don't write code that actively breaks on touch. Avoid mouse-only assumptions in places that could trivially support both.
