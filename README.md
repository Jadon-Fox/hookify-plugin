# hookify (pure upstream mirror for Grok / xAI pin)

**Source of truth:** [`anthropics/claude-plugins-official` → `plugins/hookify`](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/hookify)

This repository is a **pin-friendly copy** of pure Hookify for Grok Build / xAI Official marketplace installs. It does **not** bake host-specific absolute-load patches.

## Install

```bash
grok plugin install Jadon-Fox/hookify-plugin@<FULL_SHA> --trust
# or after catalog merge:
grok plugin install hookify --trust
```

`--trust` is required for hooks to run (Grok plugin trust gate).

## After install: add your rules (normal Hookify)

Rules are **not** shipped as your policy. Create them after install:

```bash
# via skill/command
/hookify Warn when I use rm -rf

# or files
~/.claude/hookify.<name>.local.md
# project-local (standard Hookify):
.claude/hookify.<name>.local.md
```

Stock loader looks under **project** `.claude/hookify.*.local.md` (cwd-relative). Put rules there, or use home + project as you prefer for your workflow.

## Verify

```bash
test -f ~/.grok/installed-plugins/hookify-*/hooks/stop.py && echo STOP_OK
grok plugin details hookify
```

## Upstream

See Anthropic LICENSE and README content in this tree (synced from official plugin folder).
