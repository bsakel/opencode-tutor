# opencode-tutor

### Personal coding tutor for opencode

`opencode-tutor` turns opencode into a teaching-oriented coding assistant.

It adapts to how you like to learn, remembers what you're learning, resumes topics over time, and guides you with hint-first help instead of jumping straight to solutions.

> Based on [pi-tutor](https://github.com/denismrvoljak/pi-tutor) by Denis Mrvoljak. Original idea and tutoring methodology adapted for the opencode platform.

## Features

- Adapts to your learning style and preferences
- Remembers what you're learning and resumes it later
- Keeps separate topic folders for different learning tracks
- Prefers hints, questions, and next steps before full solutions
- Helps you reflect on progress, blockers, and what to do next
- Supports learning through small projects, exercises, and repeated practice
- Pure markdown state — no hidden active-track state, no TypeScript dependencies

## Installation

Clone the repo and add it to your opencode plugins directory:

```bash
git clone https://github.com/bsakel/opencode-tutor ~/.opencode/plugins/opencode-tutor
```

Or symlink it if you want to develop locally:

```bash
ln -s /absolute/path/to/opencode-tutor ~/.opencode/plugins/opencode-tutor
```

opencode will automatically discover the plugin via `plugin.json` and register the `tutor` agent and skills.

## Quick start

Switch to the tutor agent inside opencode:

```text
/agent tutor
```

That's it — you're in tutor mode. All messages go through the tutor agent until you switch away.

To return to the default coding agent:

```text
/agent code
```

## Usage examples

### 1. First-time onboarding

```text
/agent tutor
I want to learn SQL joins through small exercises. I'm intermediate and I prefer hints over full solutions.
```

Expected outcome:
- The tutor asks only for missing onboarding details if needed
- `~/.opencode/tutor/learner-profile.md` is created
- Future tutoring turns use that profile

### 2. Create a new track

```text
/agent tutor
I want to learn Redis caching patterns
```

Expected outcome:
- If no matching track exists, the tutor creates `~/.opencode/tutor/tracks/redis-caching/`
- The new track gets `track.md`, `project.md`, `roadmap.md`, and `progress.md`
- Roadmap tasks are tracked as markdown checkboxes and mirrored into progress journey status

### 3. Resume an existing track

```text
/agent tutor
I want to keep learning SQL joins
```

Expected outcome:
- The tutor matches the saved track by name
- It reads `track.md`, `project.md`, `roadmap.md`, and `progress.md`
- Tutoring resumes from the current focus or next step

### 4. Ask for a hint

```text
give me a hint on LEFT JOIN filtering
```

Expected outcome:
- The tutor gives the next hint level instead of jumping to the full solution
- If you report meaningful progress or a blocker, `progress.md` is updated

### 5. Reflect on what happened

```text
I finished two exercises but still confuse WHERE vs ON
```

Expected outcome:
- The tutor records the reflection in `progress.md`
- Blockers and completed items are adjusted if needed
- `Next step` is refreshed

### 6. Ask what to do next

```text
what should I do next for SQL joins?
```

Expected outcome:
- The tutor reads the latest roadmap and progress context
- It refreshes `progress.md` if needed and returns one concrete next step

## State layout

All tutor data lives under `~/.opencode/tutor/`:

```text
~/.opencode/tutor/
├── learner-profile.md
└── tracks/
    └── <topic-folder>/
        ├── track.md
        ├── project.md
        ├── roadmap.md
        └── progress.md
```

The folder name is a short filesystem-safe topic slug (lowercase, hyphens, no special characters).

File roles:
- `learner-profile.md` — durable learner preferences, goals, topics, and tutoring style
- `track.md` — what the track is about, keywords, learner-specific notes
- `project.md` — concrete project brief (goal, scope, acceptance criteria, constraints, deliverables)
- `roadmap.md` — milestones and checkbox todo tasks/exercises (`- [ ]` / `- [x]`)
- `progress.md` — journey status (roadmap completion), current focus, completed work, reflections, blockers, next step

This plugin is intentionally markdown-first. There is **no hidden active-track state** to keep in sync.

## Development

### Modifying the agent

Edit `agents/tutor.agent.md` to change tutor behavior, add interaction patterns, or update the templates for state files. This single file replaces all of the pi extension logic, prompt templates, and system prompt injection.

### Adding skills

Create a new directory under `skills/<name>/` with a `SKILL.md` file. Follow the pattern in the existing skill files: frontmatter with `name`, `description`, and `disable-model-invocation: true`, then markdown instructions.

### Testing

See [TESTING.md](TESTING.md) for the verification checklist.

> The `archive/` folder contains the original pi-tutor source code for reference during development. It will be removed in a future cleanup pass once the opencode port is fully verified.

## Known limitations

- **Heuristic track matching.** Track selection is based on folder name and keywords, so overlapping topic names may be ambiguous. Name topics clearly.
- **Name the topic clearly.** Because state is markdown-first with no hidden active-track state, resume works best when the learner names the topic clearly.
- **No active-track file.** The plugin does not keep hidden current-track state between sessions.

## License

MIT

