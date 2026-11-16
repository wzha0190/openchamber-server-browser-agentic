# Local patches

A private copy of [JosueGalRe/openchamber-server-browser](https://github.com/JosueGalRe/openchamber-server-browser), kept so local changes survive updates. Based on upstream `v0.10.0`.

## agentTakesControl

`config.json`:

```json
{ "agentTakesControl": true }
```

By default a viewer who clicks the picture holds it, and every agent action is refused with "The user controls the browser. Wait for them to hand control back." until they press **Hand back to agent**. With `agentTakesControl` true, agent actions run anyway, so a click stops blocking the agent and simply acts on the page. The panel's *You have control* line is OpenChamber's own state and stays until the viewer hands back; only the waiting goes away.

Changed in `src/config.js`, `src/main.js`, `src/browser-manager.js`, `src/browser-runtime.js`, with tests in `test/config.test.js` and `test/browser-manager.test.js`, and notes in `README.md` and `docs/configuration.md`.

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
