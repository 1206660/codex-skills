# Reskin Patterns

Use these patterns when refreshing a Godot UI with existing assets.

## 1. Cardify a flat menu

When a menu is structurally correct but visually flat:

- keep the current node hierarchy
- wrap the main content in an existing `PanelContainer`
- apply an existing card or panel `StyleBox`
- tighten spacing and reinforce title hierarchy

## 2. Promote existing icons into status anchors

When the HUD has text but weak scanability:

- add existing icons to section headers or counters
- align icons consistently across ability, alert, or objective clusters
- keep icon treatment uniform instead of mixing sizes and colors

## 3. Use label settings for hierarchy

When a scene relies on default label styling:

- assign an existing title label setting to top-level headings
- assign subtitle and hint settings to secondary copy
- keep body labels visually quieter than action labels

## 4. Reuse overlays and glow layers carefully

When the UI needs more atmosphere:

- use current background gradients, glows, or shader panels sparingly
- prefer one or two large atmospheric layers over many small decorations
- keep buttons and text readable above them

## 5. Normalize repeated controls

When menus and dialogs drift apart:

- use the same button styles across main menu and dialogs
- reuse the same panel treatment for popup shells
- reuse the same spacing rhythm for vertical stacks and footers

## Stop conditions

Stop the pass when:

- the main surfaces feel coherent
- repeated controls share the same visual language
- the next changes would require new art rather than better reuse
