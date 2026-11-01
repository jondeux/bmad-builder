<!-- source: https://docs.bmad-method.org/toolsmith/distribute-your-module/ | retrieved: 2026-11-01 | converted from HTML with pandoc -->

# Distribute Your Module

Use Smithy’s package mode and a Git repository to publish a module that anyone can add with `npx skills add`. There is no separate registry or build step.

## When to Use This


- You have a skill or module ready to share with a team or the public
- You want `bmad` to check it for updates
- Several projects should use the same version

## When to Skip This


- The skill is for one project. Keep it in that project.
- The skill is not stable yet. Test it locally first.

Prerequisites

- A finished skill or set of skills
- A Git repository
- Toolsmith installed

## Steps


### 1. Package the Skills


Ask Smithy to package them. If you built them with Smithy, they are probably registered already. See [Package](/toolsmith/modes/#package).

Smithy asks for the `update_source` if it does not have one. This is where update checks look for your module. Its usual form is `github:<owner>/<repo>/<path>`, where `<path>` is the folder holding your skill folders, such as `skills`. Update checks read `bmod.toml` from the `main` branch. A member’s `source` must match its record’s `update_source`.

### 2. Lay Out the Repository


Put your skills under `skills/`. A multi-skill module looks like this:

```
your-repo/├── skills/│   ├── bmod-acme/│   │   ├── SKILL.md│   │   ├── bmod.toml│   │   ├── help/│   │   │   └── help.md│   │   └── roster.toml│   ├── acme-release-notes/│   │   ├── SKILL.md│   │   └── bmod.toml│   └── acme-agent-scout/│       ├── SKILL.md│       ├── customize.toml│       └── bmod.toml├── README.md└── LICENSE
```

A single-skill module is one folder under `skills/`, with its `help/` folder inside and no `bmod-<code>` folder. A repository can hold several modules, but `validate_manifests.py` requires all their records to share one version, and every folder under `skills/` to have a `bmod.toml`.

### 3. Validate


Ask Smithy to validate the repository. It runs `validate_manifests.py` and checks each skill. Fix what it reports before you publish. See [Validate](/toolsmith/modes/#validate).

### 4. Test a Local Install


Add the module from its folder in a separate test project, then ask `bmad` to run `bmad setup <code>`. Run `bmad status` to confirm the module and version. Check that its questions read well, and that `bmad` recommends the skills as you intended.

```
npx skills add ./path/to/your-repo
```

Terminal window

### 5. Publish


Push to your Git host and tag the release. Tell users how to add the module, and to include the record.

```
npx skills add your-org/your-repo
```

Terminal window

Then they ask `bmad` to run `bmad setup acme`. Users can pin to a tag with `your-org/your-repo#v1.2.0`. See [Add Modules](/customize/add-modules/).

### 6. Ship Updates


- **Bump `version` in the record for every release.** `bmad setup` compares it with the installed version to offer the update.
- **Add new skills to the record’s `skills` list.** Add a help entry too, and a roster entry for an agent.
- **Record renames and removals in `retired.toml`.** Setup offers to delete the old skill and moves its custom files to the new name. Never reuse a retired name.
- **Ship a migration when users’ files must change.** Add a `migration-<n>.toml` beside `bmod.toml`, and `bmad migrate` will run it.

## What You Get


- A module anyone with access can add with `npx skills add`
- Setup questions asked once per project, with answers kept in the project
- Guidance from the `bmad` skill, based on your help files
- Update checks against your source
- Clean handling of renamed and removed skills

## Tips


- Write a README with what the module does, how to add it, and any outside tools it needs.
- Add a `LICENSE`.
- Use `pre_install_message` for what users must know before an update, and `post_install_message` for what to do after installing.
- Use `required_skills` for a skill your module invokes, with a `source` and a minimum `version` for one from another repository.
- Run a [baseline eval](/toolsmith/bmad-eval/) before you publish. For a distributed skill, only that shows the skill beats the bare model on real cases.
