# Browser Style Prototype

**Browser Style Prototype** is an Antigravity plugin that turns any open browser tab into an interactive live styling canvas. Instead of guessing CSS values in source files or triggering premature code edits, the agent prototypes CSS, DOM structure, and visual layout adjustments directly in the active browser runtime first—and writes to your project codebase only after you explicitly approve the visual result.

## Core Workflow

1. **Live Browser Preview**: Attach to your specified open browser tab and apply scoped, reversible CSS and DOM adjustments in real time.
2. **Explicit User Approval Gate**: Present the visual preview and Maintain a concise *Target Prototype Spec*. Pausing at the preview stage prevents accidental source edits when you are merely exploring directions or approving browser tool calls.
3. **Idiomatic Source Implementation**: Once you explicitly approve a design, translate the runtime experiment into clean, maintainable project code (components, stylesheets, and design tokens) without dumping raw computed styles or brute-force `!important` hacks.
4. **Clean-Slate Verification**: Strip all temporary browser overrides (or reload safely) and verify that the browser renders the approved design strictly from the updated source code.

## Key Guardrails

- **Strict Preview vs. Code Boundary**: Praise like *"Looks good"* or requests for another visual tweak never trigger source file modifications. Source files are touched only when you explicitly ask to apply the design to your codebase.
- **Non-Destructive Iteration**: Tracks baseline styles and preserves unsaved user input on the page; handles client-side framework re-renders (React, Vue, etc.) without mutating business state.
- **Visual-Only Scope**: Prevents accidental form submissions, mutating API calls, or business logic side effects during layout prototyping.
- **Honest 3-Stage Reporting**: Reports **Live Preview**, **Source Implementation**, and **Clean-Slate Verification** as separate milestones so temporary browser patches never mask implementation bugs.

## Prerequisites

This plugin requires an environment with active browser inspection and live DOM/CSS editing capabilities—such as Antigravity's built-in browser tools or the **Chrome DevTools** MCP plugin.

## Usage

Invoke the skill explicitly with `/browser-style-prototype` or ask in natural language:

- *"Preview a cleaner two-column card layout directly in my open browser tab without touching source files yet."*
- *"Tweak the header spacing, font weights, and border radius live in the browser so I can compare options."*
- *"Apply the approved browser style prototype to the project components and verify the clean render."*

## Installation

Install from the Antigravity Marketplace or directly via the Antigravity CLI:

```bash
agy plugin install https://github.com/zaixiakongyiji/browser-style-prototype
```

## License

[MIT](LICENSE)
