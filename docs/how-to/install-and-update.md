# How to install and update the skill

This guide shows you how to get diataxis-docs available in your Claude Code
sessions and how to keep it current. Two routes exist: the plugin route,
which manages updates for you, and the clone route, which you update by
pulling.

## Install as a plugin

Use this route if you want update management. In any Claude Code session:

```
/plugin marketplace add PeterKnego/diataxis-docs-skill
/plugin install diataxis-docs@diataxis-docs-skill
```

The first command registers this repository as a plugin marketplace; the
second installs the skill from it. Restart the session if the skill does not
appear immediately.

## Install from a clone

Use this route if you want to read or modify the skill files, or if you work
offline. Clone the repository, then from its root:

```bash
ln -s "$(pwd)/skills/diataxis-docs" ~/.claude/skills/diataxis-docs
```

The symlink means edits and pulls in the clone take effect immediately — no
reinstall step.

If `~/.claude/skills/diataxis-docs` already exists from a previous install,
remove it first; `ln -s` will not replace it.

## Verify the install

In a new Claude Code session, type `/diataxis-docs`. If the skill is
installed, it appears in the slash-command suggestions. Plugin installs may
list it under its namespaced form, `diataxis-docs:diataxis-docs`.

## Update

- Plugin route:

  ```
  /plugin marketplace update diataxis-docs-skill
  ```

- Clone route: `git pull` in the clone. The symlink picks the changes up
  immediately.

## Switch routes

To move from clone to plugin, remove the symlink first, then follow the
plugin route:

```bash
rm ~/.claude/skills/diataxis-docs
```

To move from plugin to clone, uninstall the plugin
(`/plugin uninstall diataxis-docs@diataxis-docs-skill`), then follow the
clone route.
