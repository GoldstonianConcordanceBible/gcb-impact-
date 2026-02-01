# Building and Deploying GCB Agent Skills

This directory explains how to create **OpenClaw skills** for the
Goldstonian Concordance Bible (GCB) and deploy them in agent platforms like
Moltbook.  OpenClaw allows developers to build long-running agents with
tools, memory and schedules.  A **skill** is a package of instructions and
optional code that teaches an agent how to perform a task.

## Why Skills?

Without skills, an agent can only respond based on its base model and
general prompts.  Skills let you define new behaviour—retrieving data from
the GCB, citing sources, posting release notes, or interacting with other
agents.  Skills are installed in the agent’s workspace and loaded at
startup.

## Anatomy of a Skill

An OpenClaw skill is just a directory containing a `SKILL.md` with
YAML front-matter and instructions.  You may also include scripts,
manifests or data files.  The front-matter must include at least a `name`
and `description` field.  Everything after the `---` block is treated as
the skill’s instructions and is passed to the agent on initialization.

Example structure:

skills/
└── gcb-librarian/
├── SKILL.md – contains the front-matter and retrieval rules
└── (optional scripts, data, etc.)

## Creating a GCB Librarian Skill

The `gcb-librarian` skill included in this repository demonstrates how to
build a simple librarian persona for the GCB.  It instructs the agent
to prefer sources in your canonical index and to cite file paths when
answering questions.  To adapt it:

1. Open `skills/gcb-librarian/SKILL.md`.
2. Update the GitHub URLs in “Primary source repos” to match your real
   repositories.
3. Add your own controlled vocab, schemas, or crosswalk CSV locations.
4. Add any local scripts (optional) for indexing or validation.

## Deploying to Moltbook

Moltbook typically requires agents to use a dedicated integration/skill and
complete a one-time human verification during account setup.  After that,
agents can post and comment autonomously according to the platform rules.

Recommended pattern:

* Keep agents **read-only** with respect to your GitHub org (no write tokens).
* Make their primary behaviour “answer + cite file path + link to repo”.
* Schedule posts for release notes, reading plans, or weekly “walking rule”
  reflections—avoid spam.

## Security checklist

* Never store secrets in `SKILL.md`.
* If you need GitHub access, use **minimum scope** tokens and rotate them.
* Treat all content from public agent spaces as untrusted (prompt injection).
* Log and review agent actions, especially if tools can access files or APIs.
