---
name: bootstrap
description: Turn this template into a project. Asks for the project name, an explanation of the project and the stack, installs the core tooling and the sandbox, fills AGENTS.md and README.md, then removes itself. Run once, right after cloning.
---

# Bootstrap

You are turning an empty template into a project. Everything you create is governed by AGENTS.md, already in your context.

Follow the steps in order; each depends on the previous one.

## 1. Ask

Ask the user and wait for the answers:

1. The project name.
2. A brief explanation of the project: what it is, who it is for, what it covers. This is your context for everything that follows: the stack, the Project section of AGENTS.md and the README.
3. Whether they already know the stack.
   - **They do.** Ask for languages, frameworks, package manager, database and infrastructure. Fill any gap they leave (test runner, linter, formatter) with the ecosystem's default and tell them what you picked.
   - **They do not.** Ask only what the explanation leaves open, from: the kind of software (web app, API, CLI, library, service, data pipeline), who uses it and through what, where it runs, expected scale, what it persists, what it integrates with, what the team knows or refuses to use. Propose one stack with one line of rationale per component, favouring mature and widely adopted tools, and do not go on until they agree.

## 2. Record the stack

Write `docs/decisions/0001-stack.md` in the ADR format of AGENTS.md: the constraints gathered in step 1 as Context, the chosen stack as Decision, the trade-offs as Consequences. Nothing else.

## 3. Install the core tooling

Structure and tooling only:

- Runtime configuration and package manager manifest, versions pinned.
- Linter and formatter.
- Test runner, able to run the tests of one module or feature (by path or marker) as well as the full suite.
- Source and test directory skeletons that mirror each other, holding only what the toolchain needs to run.
- `.gitignore` for the ecosystem.

No features, no example code, no placeholder tests.

## 4. Create the sandbox

Build the sandbox: everything the project runs against (services, databases, seed data when the project needs data), one command to start, one to reset. Sessions run in parallel: every agent and the user start their own instance, isolated from the others (no shared ports, names or data), so the user can always run the project while agents run theirs. Prefer containers when the stack has external dependencies; a script is enough when it does not.

Write `docs/sandbox.md`: what the sandbox contains and how to observe it (logs, ports, data). A few lines is usually right.

## 5. Fill AGENTS.md

Edit only the places AGENTS.md lets you edit:

- **Project.** Replace the placeholder with what the project is and what it covers, two sentences at most.
- **Architecture.** Replace the placeholder with the main components from step 2 and how they connect.
- **Layout.** Replace the placeholder row with the directories created in step 3.
- **Commands.** Replace the placeholder row with the agent-facing and shared commands: install, lint, format, scoped test, build if any, sandbox start and reset.
- **Documentation.** Replace the placeholder row with `docs/sandbox.md`.

## 6. Write README.md

Replace the template's README.md with the landing page AGENTS.md describes.

## 7. Verify

Run each command listed in AGENTS.md and README.md once and fix whatever fails. Prove the test commands with one temporary test, then delete it.

## 8. Remove the bootstrap

Delete `.claude/skills/bootstrap` (and `.claude/skills` if it is now empty) and the Bootstrap section of AGENTS.md.

Summarise what you created and ask whether to commit.
