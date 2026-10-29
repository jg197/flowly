# Installing this build

This is a fork of [JasprW/flowly](https://github.com/JasprW/flowly) carrying changes that
are proposed upstream but not merged yet. The `james` branch always holds **all** of
them, so the command below does not change as more land.

```sh
paseo plugin add git:jg197/flowly --ref james --id flowly
```

To pick up later changes:

```sh
paseo plugin update flowly
```

To go back to the released plugin:

```sh
paseo plugin remove flowly
paseo plugin add jasprw/flowly
```

## Before you install

**This replaces your existing Flowly.** Two installs cannot run side by side: Flowly
resolves its data directory from a fixed plugin id rather than the installed one, so a
second copy opens the same board and the storage lock stops it. Board data is shared, so
nothing is lost either way, and reverting restores the released plugin against the same
board.

To keep the released plugin instead, install this one under a different id and disable the
other first:

```sh
paseo plugin disable flowly
paseo plugin add git:jg197/flowly --ref james --id flowly-dev
```

## What this build adds

**Shortcut as a connector.** Settings → Connections gains Shortcut, and New task gains
**From Shortcut**. The picker lists your open stories and resolves `sc-1234`, a bare
number, or a pasted story or epic link. Agents read items with `flowly_read_external`, and
an epic's Markdown lists its stories.

The connection address is your workspace: `https://app.shortcut.com/<workspace-slug>`.

**A token from the daemon environment.** Leave the token field empty and the daemon reads
`SHORTCUT_API_TOKEN` from its own environment. A typed token always wins. The value never
reaches the client.

**Following a story to its epic.** `flowly_read_external` with `epic=true` (CLI:
`external --id <artifact id> --epic`) returns the story's epic alongside it, so an agent
gets the wider goal without you attaching the epic by hand.

## Branches

| Branch | Contents |
|---|---|
| `james` | everything below, merged — **install this one** |
| `feat/shortcut-connector` | the connector alone |
| `feat/connector-secret-env` | the daemon-environment token |
| `feat/read-external-epic` | the story-to-epic hop |

Each feature branch is a candidate for its own upstream pull request, which is why they
are kept apart. `james` is rebuilt from them, so treat it as disposable: raise
changes on a feature branch, never directly on `james`.
