---
name: browser-style-prototype
description: >-
  Prototype CSS, DOM, and visual layout changes live in an open browser tab
  before modifying project source code. Use when the user asks to preview,
  tweak, or iterate on UI styling and layout directly in the browser first, and
  apply changes to source files only after explicit approval.
---

# Browser Style Prototype

Treat the active web page as an interactive visual prototype. Strictly follow the four-stage workflow:

`Live Browser Preview` → `Explicit User Approval` → `Source Code Implementation` → `Clean-Slate Verification (Temporary Overrides Removed)`

Adapt to the current harness's available browser tools (e.g., Antigravity browser tools, Chrome DevTools MCP, or page evaluation APIs) without assuming undocumented capabilities.

---

## Authorization & Safety Boundaries

- **Preview Mode by Default**: Requests to tweak styles, adjust layouts, or try visual alternatives operate strictly in the browser runtime. They do **not** authorize editing project source files.
- **What Counts as Source Approval**:
  - **NOT Approval**: Approving a browser tool call, asking for another visual iteration, or praising the preview (e.g., *"Looks good"*, *"Nice direction"*).
  - **Valid Approval**: An explicit instruction to persist the current design into the codebase (e.g., *"Apply this to the code"*, *"Update the component with these styles"*). Approval applies only to the currently selected design and scope.
- **No Source Edits During Preview**: Until explicit source approval is granted, do not modify any project files (stylesheets, components, templates, configs, or dependencies). Do not bypass the preview boundary via dev-server hot module replacement (HMR), patch application, DevTools Workspace file mapping, or standalone scratch pages. Read-only inspection of source files is permitted to understand style architecture and feasibility.
- **Visual-Only Scope**: During live preview, you may mutate runtime CSS rules, classes, DOM structure, element positioning, and visual states via supported page-editing tools or scripts. Do **not** implement business logic, submit forms, trigger mutating API calls, or alter external application data.

---

## 1. Attach to the Target Page & Verify Capabilities

1. **Use the Active Target Tab**: Attach to the existing browser tab specified by the user and confirm the target UI region. Ask for clarification only if the target cannot be determined from context. Never silently substitute a reconstructed static page, a different account session, or a new browser instance.
2. **Verify Live-Editing Support**: Check the current harness's browser tool capabilities. Confirm support for live DOM/CSS manipulation (e.g., editable DevTools Elements/Styles or runtime DOM/style injection). Read-only inspection, screenshots, click actions, or read-only `evaluate` tools are insufficient.
3. **Fail Fast When Unsupported**: This skill does not grant browser-write capabilities on its own. If live page editing is unavailable or blocked, clearly report the missing capability and stop the preview step. Do not fabricate a preview, install unauthorized tools, or fall back to editing source code.
4. **Preserve Initial Page State**: Observe the baseline visual state, viewport dimensions, and target region before making edits. Do not refresh the page just to start working—preserve the user's current session and unsaved UI state.

---

## 2. Iterate Live in the Browser

- **Scoped, Reversible Overrides**: Prefer targeted, easily reversible style overrides and minimal DOM adjustments anchored to stable selectors. Avoid brittle auto-generated class names, shifting child indices, or overly broad selectors that cause collateral visual changes.
- **Track Baseline Values**: Record original values and every runtime mutation applied during the session so you can cleanly roll back your changes without wiping out pre-existing user state.
- **Verify Visual Rendering**: After each key adjustment, inspect the actual rendered output (via screenshot or computed layout inspection) to confirm the visual change took effect. Tool execution success does not equal visual verification. Continue iterating in preview mode based on user feedback without repeatedly asking for source-code authorization.
- **Handle Framework Re-renders**: Client-side frameworks (React, Vue, Angular, etc.) may overwrite temporary DOM or class mutations on re-render. If clobbering occurs, re-observe the DOM and re-apply the necessary preview overrides—never mutate application state or edit source files prematurely to force persistence.
- **Distinguish CSS Sizing from Internal Rendering**: Resizing a container or `<canvas>` via CSS does not automatically re-layout or re-render its internal drawn content. Explicitly flag any simulated states, non-functional interactions, or rendering limitations rather than presenting them as working features.
- **Protect Against State Loss**: Refreshing, navigating, or closing the tab destroys runtime edits. Before performing any action that resets the page, record all active adjustments and use tab-preservation features if supported by the harness.

---

## 3. Maintain the Target Spec & Await Approval

Keep a concise, up-to-date **Target Prototype Spec** in the conversation context (do not create unnecessary tracking files or prototype repos unless requested):

- **Context**: Target URL/tab, modified UI region, viewport dimensions, and relevant UI state.
- **Visual Spec**: Finalized CSS properties/values, layout relationships (Flexbox/Grid/spacing), and structural DOM changes.
- **Visual Reference**: Latest preview screenshot or active browser state reference (when supported).
- **Status & Constraints**: Known interactive limitations and whether source-code modification has been authorized.

Update this spec whenever a new iteration supersedes a previous one, and preserve it across context compaction or handoffs. If context is lost to the point where the target design is ambiguous, re-confirm with the user rather than guessing.

**Gate Check**: Present the live preview and summarize the key visual changes, then **pause and wait** for explicit user approval to modify source files (unless already granted). If the user cancels, revert all temporary runtime changes without forcing a destructive full-page reload over unsaved user input.

---

## 4. Translate Approved Design to Source Code

1. **Align with Project Architecture**: Cross-reference the approved Target Prototype Spec against the project workspace. Read project conventions and existing `git diff` to respect prior work.
2. **Locate Source of Truth**: Identify the exact components, templates, stylesheets, and design tokens (CSS variables, Tailwind theme tokens, etc.) governing the target UI. Implement the approved visual changes while keeping business logic, event handlers, data bindings, and API contracts intact. If achieving the layout requires altering business logic, stop and request scope confirmation.
3. **Write Maintainable Code**: Translate browser experiments into clean, idiomatic source code:
   - Do **not** blindly copy-paste raw computed styles, runtime-generated class names, or temporary DOM wrappers.
   - Do **not** carry over brute-force preview overrides or arbitrary `!important` declarations.
   - Check the blast radius when modifying shared components or global design tokens to prevent unintended regressions on other pages.
4. **Execute Cohesively**: Complete the authorized implementation and related static checks continuously without asking for file-by-file confirmation. If the user requests new visual changes beyond the approved spec, prototype them in the browser first and obtain approval before expanding source edits.

---

## 5. Clean-Slate Verification (Strip Temporary Overrides)

- **Remove Runtime Overrides First**: Clear all temporary browser CSS/DOM injections (or reload the page safely if no unsaved user input is at risk). Confirm the browser is rendering strictly from the updated source code so temporary overrides cannot mask implementation defects.
- **Compare Against Approved Spec**: Verify the live rendered output under matching viewport dimensions, UI states, and responsive breakpoints. Run relevant project checks (typecheck, style lint, or targeted tests) proportional to the changes made.
- **Handle Remote vs. Local Environments**: If the originally inspected tab points to an un-updated remote deployment, verify the changes against a local or preview environment running the updated source code. If live verification of the updated source is impossible, explicitly state that source implementation is complete but live visual verification remains unverified—never use a stale remote page or a patched runtime page as proof of completion.
- **Report Distinct Milestones Clearly**: Conclude with a concise summary separating the three verification states:
  1. **Live Browser Preview**: Completed and approved.
  2. **Source Implementation**: Files updated and static checks run.
  3. **Clean-Slate Visual Verification**: Verified on unpatched source rendering (or explicitly marked as unverified with the reason).
