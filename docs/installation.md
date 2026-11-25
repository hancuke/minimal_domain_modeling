# Installation

[Back to README](../README.md)

Install only the skills you need. This guide covers installation, activation,
dependencies, and upgrades; see [Maintaining](maintaining.md) for repository
organization and reference-loading rules.

## Install from GitHub (Recommended)

The target machine needs Node.js, npm, and Git. Run these commands from the root of the target business project:

```sh
# List available skills in the repository without installing them
npx skills add hancuke/minimal_domain_modeling --list

# Install the domain-modeling skill and select the target agent interactively
npx skills add hancuke/minimal_domain_modeling --skill minimal-domain-modeling
```

Installation targets the current project by default. To install in Claude Code's user-level directory for use across all projects:

```sh
npx skills add hancuke/minimal_domain_modeling \
  --skill minimal-domain-modeling \
  --agent claude-code \
  --global
```

Replace the `--skill` value with `communication-policy`, `code-structure-review`, `implement`, or `ui-design` to install another skill. When installing `software-design`, also install its reference dependency and the skill that handles implementation:

```sh
npx skills add hancuke/minimal_domain_modeling --skill code-structure-review
npx skills add hancuke/minimal_domain_modeling --skill software-design
npx skills add hancuke/minimal_domain_modeling --skill implement
```

Choose the same agent and installation scope for all three installations so the directories share one skills parent directory. Do not assume the installer resolves cross-skill dependencies automatically. After installation, check that the local state-and-transitions reference and all three cross-skill references in `software-design/SKILL.md` are readable and that the agent can find `implement`. Report missing dependencies explicitly; rules that were not loaded must not be treated as applied.

`npm install skills` installs only the Skills CLI, not this repository's skills. This repository does not need to be published to npm.

GitHub installations use pushed files. Private repositories require Git credentials with access on the target machine. Before updating an existing installation, compare versions and local changes to avoid overwriting them.

See the [Skills CLI documentation](https://github.com/vercel-labs/skills/blob/main/docs/commands.md) for installation details.

## Activate Communication Policy After Installation

For a project-level installation, run this from the target project's root:

```sh
npx skills add hancuke/minimal_domain_modeling \
  --skill communication-policy
```

For a user-level installation shared across projects:

```sh
npx skills add hancuke/minimal_domain_modeling \
  --skill communication-policy --global
```

Select the target agent during installation. Installation does not activate the skill, and a global installation does not guarantee automatic loading for every task. For occasional use, load it using the target agent's invocation mechanism. For persistent use, merge the following example into the target project's `AGENTS.md`, provided the agent supports and reads that file:

```markdown
## Communication Policy

Before composing user-facing responses or documents, load and apply the
installed communication-policy skill if it is not already loaded in this
session. Reload it if its content changes.

Apply it alongside task-specific skills. Preserve required content, formats,
verification requirements, and approval gates. Do not impose one universal outline.

If the skill is unavailable, report the missing dependency and do not claim
that the policy has been applied.
```

This example is only for post-installation activation; this repository does not create an actual `AGENTS.md`. The installation command does not write these standing instructions for you. For other agents, place the entry in a supported standing-instructions file and configure a readable skill path that matches the actual installation. Persistent user-level activation likewise requires corresponding user-level instructions. Do not rely on the nonstandard `alwaysApply` field to guarantee activation across agents.

After configuration, start a new session and confirm that the agent can find and read the policy while still honoring task-specific delivery requirements. Successful installation alone does not establish that the policy is active.

## Copy Manually

Copy this project to the target machine and run the following user-level installation commands from this repository's root. If a destination directory already exists, compare differences first to avoid overwriting local changes.

Claude Code user-level installation:

```sh
mkdir -p "$HOME/.claude/skills"
test ! -e "$HOME/.claude/skills/minimal-domain-modeling" &&
  cp -R skills/minimal-domain-modeling "$HOME/.claude/skills/"
```

Install the software-design and implementation skills together with their shared references under the same parent directory:

```sh
mkdir -p "$HOME/.claude/skills"
test ! -e "$HOME/.claude/skills/code-structure-review" &&
  test ! -e "$HOME/.claude/skills/software-design" &&
  test ! -e "$HOME/.claude/skills/implement" &&
  cp -R skills/code-structure-review skills/software-design skills/implement "$HOME/.claude/skills/"
```

For a Claude Code project-level installation, run this from the target business project's root and replace the source path with this project's actual location:

```sh
mkdir -p .claude/skills
test ! -e .claude/skills/minimal-domain-modeling &&
  cp -R /path/to/minimal_domain_modeling/skills/minimal-domain-modeling .claude/skills/
```

For project-level installation of the software-design and implementation skills, likewise place all three complete directories under `.claude/skills/`.
The commands above stop copying if a directory with the same name already exists; they are not update commands.
After starting a new session, invoke `/minimal-domain-modeling` explicitly or trigger it with a relevant modeling request.

For other tools that support Agent Skills, copy the same directory into the tool's designated skill search path. Consult the target tool's documentation for installation locations and explicit invocation syntax.

See the [Claude Code Skills documentation](https://code.claude.com/docs/en/skills) for installation details.

## Upgrade Older Installations

Compare the installed version and local changes before updating. Confirm that the new skill directories and required references are available before removing old installations.

The structure-review entry point is now `code-structure-review`, replacing `module-deepening` (previously `codebase-design`). When updating an older installation, first check local changes and confirm that the new entry point and shared references are available. Then remove the old entry point to avoid discovering multiple sets of automatic trigger rules. The former `references/deepening.md` is now `references/dependencies-and-verification.md`; update local custom references accordingly. No compatibility entry point is retained under the old name.

The software-design entry point has been renamed from `implementation-design` to `software-design`. When updating, confirm that the new entry point and shared references are available before removing the old installation; do not retain both automatic entry points.
