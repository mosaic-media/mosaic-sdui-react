# @mosaic-media/sdui-react

The **React runtime** for the [Mosaic](https://github.com/mosaic-media) Server-Driven-UI contract — the web binding that turns an SDUI payload into rendered UI. Consumed by the Shell ([`mosaic-shell`](https://github.com/mosaic-media/mosaic-shell)), the component storybook ([`mosaic-storybook`](https://github.com/mosaic-media/mosaic-storybook)), and any other web surface — all as peers.

It is a **client implementation**, not the contract. The technology-agnostic contract (schema, standard definitions, tokens) lives in [`@mosaic-media/sdui`](https://github.com/mosaic-media/mosaic-sdui); this is one specific way to render it, in React. That's why it's **AGPL-3.0-only** (first-party client code) while the contract is Apache-2.0.

## What's in it

- **Primitives** — `Box`, `Text`, `Image`, `Icon`, `Pressable`, the form inputs, `Tabs`, `Menu`, … the irreducible, client-implemented vocabulary, styled from tokens.
- **Registry + renderer** — `register`/`resolve`, `RenderNode` (recursive tree walk), and the `Unknown` fallback for open-vocabulary forward-compat.
- **Definition expander** — `defineComponent` + the `$bind`/`$match`/`$each`/`$if`/`Outlet` template engine, so components delivered as data render like native ones.
- **Runtime** — `ShellProvider` (interprets `Action` envelopes, owns overlays + toasts), `useRuntime`, `OverlayHost`/`ToastHost`, and a GraphQL client.
- **The token-driven skin** — shipped as `@mosaic-media/sdui-react/styles.css`.

## Use it

```bash
npm install @mosaic-media/sdui-react react react-dom
```

```tsx
import {
  ShellProvider, RenderNode, installComponents, defineComponents,
} from "@mosaic-media/sdui-react";
import "@mosaic-media/sdui-react/styles.css";

installComponents();                 // register the built-in vocabulary
// defineComponents(moduleDefinitions) // + any definitions delivered as data

<ShellProvider screen="home" onNavigate={…} onBack={…} render={…}>
  <RenderNode node={payload} />
</ShellProvider>
```

For local cross-repo work (developing the runtime against a consumer), a
consumer can depend on it by path — `"@mosaic-media/sdui-react": "file:../mosaic-sdui-react"` — instead of the published version; the `prepare` script builds `dist` on install.

## Build

```bash
npm install      # runs prepare → build
npm run build    # tsc → dist + copy the CSS
npm run typecheck
```

Pure `tsc` build (source ships nothing; consumers get `dist` JS + `.d.ts`). React is a peer dependency.

## Releasing

`git tag v0.1.0 && git push origin v0.1.0` → `.github/workflows/release.yml` publishes to npm (needs an `NPM_TOKEN` secret for the `@mosaic-media` scope).

## Licence

**AGPL-3.0-only** (see [`LICENSE`](LICENSE)). See [ADR 0022–0024](https://github.com/mosaic-media/mosaic-architecture/tree/main/docs/adr) for the first-party-client licensing rationale.
