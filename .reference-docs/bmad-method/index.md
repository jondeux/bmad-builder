<!-- source: https://docs.bmad-method.org/ | retrieved: 2026-11-01 | converted from HTML with pandoc -->

# Turn ideas into software. At any scale.

Think it through, then build it. You make the calls, so you understand what you ship.

[Build your first change](./start/build-your-first-change/) [Install BMad](./start/install-bmad/)

BMad adds a set of named commands, called skills, to AI coding tools such as Claude Code and Cursor. Some of them help you think: explore an idea, research it, argue against it, and write down what you have settled on. Others help you build: give `bmad-build` a change you want made, and it writes the code and reviews it.

You can use either group on its own. Many people run the thinking skills and never ask BMad to write a line of code, and a small fix can go straight to building with no planning at all.

## Find Your Starting Point


Every path runs the same loop. Bigger work enters it earlier and goes round it more often; it does not become a different way of delivering.

**You are not sure how much process the change needs.** [Choose a Planning Path](/plan/choose-a-planning-path/).

**You want to see it work.** [Build Your First Change](/start/build-your-first-change/) walks through one build in an empty project.

**You know exactly what needs to change, and it is small.** Run `bmad-build` and describe the change. See [Build a Change](/build/build-a-change/).

**You are working in an existing codebase.** Consider running `bmad-project-context`, then build as usual. See [Start in an Existing Codebase](/existing-codebases/start-in-an-existing-codebase/) and [Set and Maintain Project Context](/existing-codebases/set-and-maintain-project-context/).

**You are building a larger feature or a whole product.** If you can give Sofia, the Product Owner (`bmad-agent-po`), a complete intent, start there. If you need to go through the ideation/planning paces first, choose a path in [Choose a Planning Path](/plan/choose-a-planning-path/).

**Your idea is still vague, or you are not sure it is a good one.** Generate options or [pressure-test the idea](/plan/explore-and-validate-an-idea/), then gather evidence with [Deep Recon](/plan/research-a-decision/).

**You want BMad to follow your team’s own rules and practices.** See [Customize BMad](/customize/customize-bmad/) and [Adopt BMad Across a Team](/customize/adopt-bmad-across-a-team/).

Unsure where to start?

Run `bmad`. If that is not enough, see [Get Answers About BMad](/start/get-answers-about-bmad/).
