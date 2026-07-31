# hookify (Grok Build / xAI marketplace)

Configurable **hooks** driven by markdown rules (`hookify.*.local.md`).

Upstream concept: Anthropic Hookify. This host package is maintained for **Grok Build** with a critical fix:

> Rules load from **absolute `~/.claude/hookify.*.local.md` first**, then optional project `.claude/`.  
> So when the agent cwd is `training_orchestrator` (or any tree without local hookify files), home rules still apply.

## Install (xAI Official marketplace — after catalog merge)

```bash
# Marketplace already on most Grok installs as "xAI Official"
grok plugin marketplace update
grok plugin install hookify --trust
```

`--trust` is required so **hooks actually run** (skills alone are not enough).  
That is the trust gate in Grok docs (`09-plugins.md`) — not OAuth for hookify itself. GitHub MCP (if used elsewhere) uses a PAT; marketplace install uses your normal `gh`/git credentials.

## Install (direct pin — reproducible)

```bash
grok plugin install Jadon-Fox/hookify-plugin@<FULL_40_CHAR_SHA> --trust
# or
grok plugin install https://github.com/Jadon-Fox/hookify-plugin.git --trust
```

With `[marketplace] require_sha = true`, always use the full pin from the catalog.

## Install (local path)

```toml
# ~/.grok/config.toml
[plugins]
paths = ["/home/jfox/src/hookify-plugin"]
enabled = ["hookify"]
```

```bash
grok plugin install /home/jfox/src/hookify-plugin --trust
```

## Rules location (host)

| Priority | Path |
| --- | --- |
| 1 (always) | `~/.claude/hookify.*.local.md` |
| 2 | `$CLAUDE_CONFIG_DIR/hookify.*.local.md` |
| 3 | `~/.grok/hookify/` and `~/.grok/hooks/` |
| 4 (override by name) | `$CWD/.claude/hookify.*.local.md` |

## After marketplace reinstall

Marketplace reinstalls can overwrite `core/config_loader.py`. Re-apply host patch if needed:

```bash
cp -a ~/.grok/hookify-patches/config_loader.py.absolute-load-20260731 \
  ~/.grok/installed-plugins/hookify-*/core/config_loader.py
# Or reinstall from this pinned repo (absolute load is already baked in).
```

## Verify

```bash
test -f ~/.grok/installed-plugins/hookify-*/hooks/stop.py && echo STOP_OK
# From orch cwd:
cd /path/to/training_orchestrator
python3 -c "import sys; sys.path.insert(0,'.'); ..."  # or: next tool use loads n rules from home
grok plugin details hookify
grok inspect --json | rg hookify
```

## What this is not

- Not training_orchestrator / ONE_PRODUCT product locks  
- Not a substitute for look-up honesty on seals  

## License

See LICENSE (upstream Anthropic original + host modifications).
