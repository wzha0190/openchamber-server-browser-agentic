# Local patches

A private copy of [JosueGalRe/openchamber-server-browser](https://github.com/JosueGalRe/openchamber-server-browser), kept so local changes survive updates. Based on upstream `v0.10.0`. Private home: [wzha0190/openchamber-server-browser-agentic](https://github.com/wzha0190/openchamber-server-browser-agentic).

## agentTakesControl

`config.json`:

```json
{ "agentTakesControl": true }
```

By default a viewer who clicks the picture holds it, and every agent action is refused with "The user controls the browser. Wait for them to hand control back." until they press **Hand back to agent**. With `agentTakesControl` true, agent actions run anyway, so a click stops blocking the agent and simply acts on the page. The panel's *You have control* line is OpenChamber's own state and stays until the viewer hands back; only the waiting goes away.

Changed in `src/config.js`, `src/main.js`, `src/browser-manager.js`, `src/browser-runtime.js`, with tests in `test/config.test.js` and `test/browser-manager.test.js`, and notes in `README.md` and `docs/configuration.md`.

## idleMinutes

`config.json`:

```json
{ "idleMinutes": 0 }
```

Upstream closes a chat's browser scope after five idle minutes, removing its profile and sign-ins. `idleMinutes` sets that limit; `0` never expires a scope, so a sign-in survives as long as the service process lives. OpenChamber's own ten-minute stop of an unattended provider is separate: this copy also declares `service.keepRunning` in `package.json`, so the host keeps the service up and does not wipe profiles on that timer. The four-scope bound and its least-recently-used eviction are unchanged.

Changed in `src/config.js`, `src/main.js`, with tests in `test/config.test.js`, notes in `README.md` and `docs/configuration.md` and `docs/how-it-works.md`, and the manifest change in `package.json`.

## Updating from upstream

The installed copy is `/srv/openchamber/data/extensions/server-browser`. Its `origin` is upstream, which is what OpenChamber's updater follows; this repository is the remote `github`.

```bash
cd /srv/openchamber/data/extensions/server-browser
git fetch origin --tags
git rebase v0.10.1            # the new upstream tag, replays the local patch
npm install && npm run build  # service/main.js is a build artifact
npm test
git push github main --force-with-lease
```

Then restart the extension in OpenChamber's Settings. If upstream later merges an equivalent option, drop the local commit and keep only the `config.json` key.
