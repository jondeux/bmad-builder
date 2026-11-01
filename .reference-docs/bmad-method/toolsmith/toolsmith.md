<!-- source: https://docs.bmad-method.org/toolsmith/toolsmith/ | retrieved: 2026-11-01 | converted from HTML with pandoc -->

# Toolsmith

Toolsmith is the module for authoring BMad content. Its agent, Smithy (`bmad-toolsmith`), builds a skill, an agent, or a module from a conversation about what you need. `bmad-eval` measures whether a skill works. Use Toolsmith when you want an agent to do a job the same way every time, and when you want to share that job with a team or publish it.

A skill is a folder with a `SKILL.md` file. Your coding tool reads the description in that file to decide when to load the skill, and the agent follows the rest of the file once it does. Everything Toolsmith builds starts from that folder.

## Install Toolsmith


Toolsmith installs from the BMAD-METHOD repository. If you do not have BMad yet, start with [Install BMad](/start/install-bmad/). Then add the module from your project folder:

```
npx skills add bmad-code-org/BMAD-METHOD --skill bmad-toolsmith --skill bmad-eval --skill bmod-toolsmith
```

Terminal window

Ask the `bmad` skill to run `bmad setup toolsmith`. The module asks no setup questions.

## Talk to Smithy


Start the `bmad-toolsmith` skill, or ask for Smithy by name, and say what you want. You do not need to know the shape of the result, or which mode to use. Smithy shows a menu when you give no request.

| Code       | What it does                                                                  |
|------------|-------------------------------------------------------------------------------|
| `BUILD`    | Build a new skill, agent, or module from a conversation                       |
| `SCAFFOLD` | Lay out a skill you will write yourself                                       |
| `LOGS`     | Propose a skill from your session logs, then build it                         |
| `EDIT`     | Change, fix, or extend a skill                                                |
| `CONVERT`  | Rebuild a skill, prompt, or command from another tool as a BMad skill         |
| `REVIEW`   | Review a skill and write a report of findings                                 |
| `CUT`      | Cut a skill’s prose to what changes what the model does                       |
| `PACKAGE`  | Package one skill or several into a module                                    |
| `VALIDATE` | Check a skill or module for what would stop it installing, loading, or firing |
| `MIGRATE`  | Convert an old-format module to the current format                            |
| `EVAL`     | Run, benchmark, or trigger-test a skill with `bmad-eval`                      |

## How a Build Works


Every new skill follows the same path. If you use the scaffold approach to write the skill yourself, steps 3 and 4 are yours.

1.  **Discovery.** Smithy reads what you have already said and asks only for what is missing, one or two questions at a time.
2.  **Smithy execution plan.** Smithy shows the name, purpose, description, shape, registration, approach, location, and files. Nothing is written until you approve it, and you can change any line.
3.  **Build.** Smithy writes the skill, and tries it on a real input when you have one.
4.  **Review.** Five review lenses read everything Smithy wrote. Smithy fixes the defects they find and tells you what changed.
5.  **Hand-off.** Smithy validates the skill, offers a trigger eval, and tells you where the skill is and what to try first.

## In This Section


| Page                                                               | Read it when you want to                                 |
|--------------------------------------------------------------------|----------------------------------------------------------|
| [Build Your First Skill](/toolsmith/build-your-first-skill/)       | Go from an idea to a working skill, then a module        |
| [Ways to Build a Skill](/toolsmith/approaches/)                    | Choose how a build runs                                  |
| [Shapes of a Skill](/toolsmith/shapes/)                            | See what Smithy can make                                 |
| [Work on an Existing Skill](/toolsmith/modes/)                     | Edit, convert, review, cut, package, or validate a skill |
| [Write a Good Skill](/toolsmith/write-a-good-skill/)               | Learn the standard Smithy writes to and reviews against  |
| [Subagent Patterns](/toolsmith/subagent-patterns/)                 | Spread a skill’s work across subagents                   |
| [Make a Skill Customizable](/toolsmith/make-a-skill-customizable/) | Let teams change a skill without forking it              |
| [Agents with Memory](/toolsmith/agents-with-memory/)               | Build an agent that remembers you between sessions       |
| [Modules and Registration](/toolsmith/modules/)                    | Decide how a skill registers with BMad                   |
| [Distribute Your Module](/toolsmith/distribute-your-module/)       | Publish a module so others can install it                |
| [Migrate an Old Module](/toolsmith/migrate-an-old-module/)         | Bring a `module.yaml` module to the current format       |
| [Eval Runner](/toolsmith/bmad-eval/)                               | Measure whether a skill works                            |

The exact file formats are in [Module Files](/reference/module-files/) and [Eval Format](/reference/eval-format/).
