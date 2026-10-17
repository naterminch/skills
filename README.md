# Skills

This folder is a collection of custom agent skills.

An agent skill is a reusable instruction set that teaches an AI agent how to perform a specific task. Each skill lives in its own subfolder and is defined by a `SKILL.md` file. The file starts with YAML frontmatter (`name`, `description`) followed by Markdown instructions the agent follows when the skill is invoked.

## Folder layout

```
skills/
  <skill-name>/
    SKILL.md
  README.md
```

- `skills/` - the root that holds every skill you create.
- `skills/<skill-name>/` - one folder per skill, named after the skill.
- `skills/<skill-name>/SKILL.md` - the skill definition (frontmatter + instructions).
- `skills/README.md` - this file, documenting the collection.

## What this folder is for

- Keep all your custom skills in one place so they are easy to find, edit, and share.
- Version and back up your skills together instead of scattering them across tool-specific directories.
- Share the whole folder (or individual skill folders) with others or across machines.

## How to add a new skill

1. Create a folder under `skills/` named after the skill, e.g. `skills/my-skill/`.
2. Create `skills/my-skill/SKILL.md`.
3. Add YAML frontmatter at the top:

   ```markdown
   ---
   name: my-skill
   description: What the skill does and when to use it.
   ---
   ```

4. Write the skill instructions in Markdown below the frontmatter.

## How to use a skill

Skills are discovered by agent tools from specific roots. To make a skill here usable, copy or symlink its folder into one of those roots:

- `~/.agents/skills/<skill-name>/`
- `~/.claude/skills/<skill-name>/`
- `<project>/.opencode/skills/<skill-name>/`
- `<project>/.claude/skills/<skill-name>/`

Example (PowerShell):

```powershell
Copy-Item -Recurse D:\projects\skills\diagram C:\Users\il\.agents\skills\diagram
```

## Skills included

- `diagram` - Convert long, dense, or complex text into a Mermaid diagram. Renders to SVG via pele. Invoke with `/diagram`.

## Install with npx skills add

This folder is compatible with the `skills` CLI, so it can be installed with:

```sh
# From a local path
npx skills add D:\projects\skills

# From a GitHub repository (after pushing this folder to GitHub)
npx skills add <owner>/<repo>

# Install a specific skill only
npx skills add D:\projects\skills --skill diagram

# Install globally (user-level) without prompts
npx skills add D:\projects\skills --skill diagram -g -y
```

Verified locally: `npx skills add . --list` discovers the `diagram` skill, and a full install succeeds across agents (OpenCode, Claude Code, Cursor, and more).