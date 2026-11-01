<!-- source: https://docs.bmad-method.org/existing-codebases/getting-deeper/ | retrieved: 2026-11-01 | converted from HTML with pandoc -->

Existing Codebases

# Getting Deeper

You already know Build from small projects. Here, you will use it in a specific version of Django: first for one bounded command change, then for three related stories under one epic. The two exercises demonstrate the [one-session and epic-sized planning paths](/plan/choose-a-planning-path/).

Prerequisites

Use a macOS or Linux shell with Git, Node.js 20.12+ and `npx`, [uv](https://docs.astral.sh/uv/getting-started/installation/), and a coding tool supported by BMad. Complete [Build Your First Change](/start/build-your-first-change/) before continuing. The exact install and launch commands below are for Claude Code. If you use another supported tool, you can run Build there instead.

## 1. Check Out the Exact Django Version


Clone Django 5.2.4 into a new directory, confirm that you have the expected source code, and create a branch for the exercise:

```
git clone --depth 1 --branch 5.2.4 https://github.com/django/django.git bmad-djangocd bmad-djangogit rev-parse HEADgit switch -c bmad-getting-deeper
```

Terminal window

`git rev-parse HEAD` should print:

```
c941d0deec0ea08a30670be0fac879f2372f071b
```

## 2. Set Up Django for Editing


Set up Python 3.12, install your Django checkout so the example app uses it, and create a small Django project next to the repository:

```
uv python install 3.12uv venv --python 3.12uv pip install -e .mkdir ../bmad-django-appuv run django-admin startproject tutorial_project ../bmad-django-app
```

Terminal window

## 3. Check the Starting Behavior


Confirm that JSON output is not yet available:

```
uv run python ../bmad-django-app/manage.py diffsettings --output=json
```

Terminal window

The command ends with this error:

```
manage.py diffsettings: error: argument --output: invalid choice: 'json' (choose from hash, unified)
```

## 4. Install BMad


Install BMad Method with the skills CLI, then let the `bmad` skill set up the project:

```
npx skills add bmad-code-org/BMAD-METHOD
```

Terminal window

Open Claude Code in this directory and ask the `bmad` skill to run `bmad setup`. Restart Claude Code once the skills are installed.

Tell Git to ignore the BMad files and uv lockfile created for this tutorial:

```
cat >> .git/info/exclude <<'EOF'/_bmad//_bmad-output//.claude//uv.lockEOF
```

Terminal window

## 5. Build It


Open your coding tool from the repository root. For Claude Code, run:

```
claude
```

Terminal window

```
/bmad-build Add JSON output support to django-admin diffsettings. Preservethe existing output formats, add focused tests, and update the commanddocumentation. Leave the implementation in the working tree for localinspection.
```

Build asks any questions it needs before it writes a plan. Answer according to your own preferences for the new JSON output. There is no single required JSON design for this exercise.

Build presents a plan and waits for you to approve it or ask for changes. Once approved, it builds and reviews the change, handles its findings, and shows you the result. Keep this exercise about JSON output for `diffsettings`; filtering, redaction, and CI behavior belong in the next exercise.

Build ends with a short summary and offers the next steps. Continue with the manual checks below before asking it to create a PR.

## 6. See It Work


Back in your shell, run Django’s `diffsettings` tests:

```
uv run python tests/runtests.py admin_scripts.tests.DiffSettings --verbosity 1
```

Terminal window

The tests should pass.

Now run the command again:

```
uv run python ../bmad-django-app/manage.py diffsettings --output=json
```

Terminal window

Look through the JSON and compare it with the choices you made with Build.

## 7. You Built It


Congratulations, you’ve now added something useful to a complex open-source codebase.

## 8. Write a Spec for the Larger Change


The next change needs three Build runs. `/bmad-forge-idea` can help you decide what to build. `/bmad-advanced-elicitation` can help you improve a draft. You do not need either here because the requirements are already clear. Send them straight to Sofia, the Product Owner:

```
/bmad-agent-po Create an epic named diffsettings-audit and break it intoexactly three stories in this order: filters, redaction, then CI status.
Read the current diffsettings implementation, focused tests, and commanddocumentation before writing the requirements. Keep every existing output formatand the JSON design already approved. Add repeatable --include and --excludeshell-glob filters. Include patterns are OR, and exclusions always win. Addrepeatable --redact shell-glob masks that replace current and default valueswith [REDACTED] without changing whether a difference exists. Add--fail-on-difference, which exits 1 when differences remain after filtering and0 otherwise. Each story adds focused tests and updates the existing commanddocumentation. Do not add another Django documentation file or an externalservice. Use diffsettings-audit as the epic's slug.
```

Sofia first has you pick or create an initiative, then writes `_bmad-output/initiative-<slug>/tickets/epic-diffsettings-audit/epic-diffsettings-audit.md` with its Requirements, and its three ordered entries in the epic’s `tickets/tickets.toml`. Read the epic file and entries and answer any questions. Continue when they match the requirements above; note the epic folder she creates.

## 9. Build the Three Stories


Run Build once for each story, in order. Complete each Build run before moving to the next one. Every run uses the same spec. You will run these stories attentively because they establish how filtering, redaction, and exit behavior fit together. Later epics with stable, repeated patterns may be better candidates for automation.

### Story 1: Filters


```
/bmad-build Implement story 1, filters, fromthe epic-diffsettings-audit ticket tree.
```

After Build finishes, observe the result:

```
uv run python ../bmad-django-app/manage.py diffsettings \  --include=DATABASES --include=DEBUG --include=SECRET_KEY \  --exclude=DATABASESprintf 'exit: %s\n' "$?"
```

Terminal window

The output has `DEBUG` and `SECRET_KEY`, but no `DATABASES`, followed by `exit: 0`. The include patterns are combined, while the exclusion wins.

### Story 2: Redaction


```
/bmad-build Implement story 2, redaction, fromthe epic-diffsettings-audit ticket tree.
```

Observe the unified output:

```
uv run python ../bmad-django-app/manage.py diffsettings \  --output=unified --include=SECRET_KEY --redact='SECRET*'printf 'exit: %s\n' "$?"
```

Terminal window

The secret does not appear. Both sides of the difference are masked:

```
- SECRET_KEY = [REDACTED]+ SECRET_KEY = [REDACTED]exit: 0
```

### Story 3: CI Status


```
/bmad-build Implement story 3, CI status, fromthe epic-diffsettings-audit ticket tree.
```

Observe a difference that remains after filtering:

```
uv run python ../bmad-django-app/manage.py diffsettings \  --include=DEBUG --fail-on-differenceprintf 'exit: %s\n' "$?"
```

Terminal window

The `DEBUG` difference remains visible, and the command finishes with `exit: 1`.

## 10. See the Whole Change Work


Now combine the three stories in one observation:

```
uv run python ../bmad-django-app/manage.py diffsettings \  --output=json --include=DEBUG --include=SECRET_KEY --exclude=DEBUG \  --redact='SECRET*' --fail-on-differenceprintf 'exit: %s\n' "$?"
```

Terminal window

The JSON contains only `SECRET_KEY`. Every current or default value exposed by the JSON shape you chose earlier is `[REDACTED]`; neither original value appears. The underlying values still differ, so the final line is `exit: 1`.

The first exercise gave Build one bounded change directly. This exercise gave three separate Build runs one spec. Filtering, redaction, and CI status still work together at the end. You have extended a mature Django command, and the final result still does what you asked for at the start.

If you want several perspectives on the result, `/bmad-party-mode` is an optional final step. You do not need it to finish this tutorial.

## 11. Review the Epic


Run Retrospective against the epic:

```
/bmad-retrospective epic-diffsettings-audit
```

Retrospective reads the epic’s `tickets.toml` and joined plans, then checks the integrated result against its requirements and source spec. It writes `epic-diffsettings-audit-retrospective.md` directly in the epic folder. Review its evidence, acceptance verdict, and proposed follow-up work. Keep the plans: they own status and historical evidence.

## 12. Keep Building


Now [install BMad in your own repository](/start/install-bmad/), then use the `bmad-build` skill to make a change you want. See [Build a Change](/build/build-a-change/) for the attended path. Use [Choose a Planning Path](/plan/choose-a-planning-path/) to decide when a change needs a spec, automation, or the full project flow.
