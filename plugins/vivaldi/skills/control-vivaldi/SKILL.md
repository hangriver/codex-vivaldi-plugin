---
name: control-vivaldi
description: "Control the user's Vivaldi browser through the official ChatGPT extension when work depends on open tabs or signed-in browser state."
---

# Vivaldi browser control

Use this skill only when the user chooses Vivaldi or the task clearly depends
on their existing Vivaldi tabs or authenticated session. Prefer a dedicated
connector for semantic operations when one is available and sufficient.

## Runtime

This community plugin does not redistribute OpenAI browser software. Load the
browser runtime from the user's installed official Chrome plugin:

```js
if (globalThis.agent?.browsers == null) {
  const runtimePath = `${nodeRepl.homeDir}/.codex/plugins/cache/openai-bundled/chrome/latest/scripts/browser-client.mjs`;
  const { setupBrowserRuntime } = await import(runtimePath);
  globalThis.agent = await setupBrowserRuntime();
}
```

Use only the persistent JavaScript execution tool for this runtime. Do not
inspect cookies, local storage, profiles, saved passwords, or session stores.

## Select Vivaldi

First try the native selector. On first selection, emit and read the complete
browser documentation before interacting with tabs:

```js
if (globalThis.vivaldi == null) {
  globalThis.vivaldi = await agent.browsers.get("vivaldi");
  nodeRepl.write(await vivaldi.documentation());
}
```

If the native selector says Vivaldi is unavailable, read
`bootstrap-troubleshooting`, then list browser connections once. Some Vivaldi
installations are reported as a Chrome-family extension connection.

Use that compatibility path only when exactly one entry has both
`type: "extension"` and `family: "chrome"`. Obtain it by its returned opaque
ID—not by a generic extension selector—and emit its complete documentation:

```js
const available = await agent.browsers.list();
const candidates = available.filter(
  ({ family, type }) => family === "chrome" && type === "extension",
);
if (candidates.length !== 1) {
  throw new Error("Vivaldi connection is unavailable or ambiguous");
}
globalThis.vivaldi = await agent.browsers.get(candidates[0].id);
nodeRepl.write(await vivaldi.documentation());
```

Before modifying a webpage through a Chrome-labeled connection, call
`vivaldi.user.openTabs()` and verify that its visible tab titles and URLs match
Vivaldi context supplied by the user. Explicit user confirmation that the
connection is their Vivaldi window is sufficient for the remainder of that
task. If the evidence conflicts or multiple candidates exist, stop and ask the
user to identify the connection; a read-only screenshot may be used.

Once verified, keep and reuse the same `vivaldi` browser binding. Claim tabs
only from fresh `vivaldi.user.openTabs()` results, and follow the selected
browser's complete documentation for interaction, safety confirmations,
screenshots, and tab cleanup. Preserve the runtime's original Chrome label in
diagnostic reports rather than claiming its family changed.

