# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Canonical documentation

This is one of seven Budibase plugin forks that share an identical build, release, and upgrade setup. **The full documentation lives in `../budibase-interval-plugin/CLAUDE.md`** — read it before changing the build, the release workflow, or the `svelte` version. It covers the rollup pipeline, the `schema.json` ↔ props contract, the release mechanics, and the mandatory rebuild procedure after a Budibase upgrade.

Only the facts specific to this repo are below.

## This plugin

A collapsible side navigation, driven either by a data provider or by a JSON definition. Fork of `poirazis/bb-component-SuperSideNavigation` (unmaintained since 2023).

- Plugin name / component key: `bb-component-SuperSideNavigation` → `plugin/bb-component-SuperSideNavigation`
- Friendly name in the builder: "Super Side Navigation"
- Current version: 1.4.1 · branch `main`
- `hasChildren: true` — renders `<slot />` when `structureSource` is `custom`
- Components: `src/Component.svelte`, `src/lib/Section.svelte`

## Repo-specific notes

- **"Error Parsing JSON Definition" is a configuration state, not a bug.** The component `JSON.parse`s the `staticStructure` setting and renders that message when it is empty or invalid. Populate the setting with a `{"sections":[{"sectionKey":…,"sectionValue":…,"items":[{"itemKey":…,"itemValue":…}]}]}` structure.
- The component tries to self-populate a sample via `builderStore.actions.updateProp("staticStructure", …)`. That API still exists in Budibase 3.x, but it only takes effect inside the builder preview — in a served app the fallback silently does nothing, so the setting must actually be saved.
- **Styling comes entirely from Budibase.** The markup uses Adobe Spectrum classes (`spectrum-SideNav`, `spectrum-SideNav-item`, `spectrum-SideNav-heading`) that this plugin never defines. If it renders as an unstyled bullet list, Budibase has stopped shipping those classes — the fix is to vendor the CSS into the component, not to touch the Svelte.
- Reads a `loading` context and `builderStore` from the `sdk` context, so any harness testing it outside Budibase must supply both as real stores.
- Upstream is unmaintained but its author is still active on other `bb-component-*` repos, so an upstream fix is not impossible.
