# Testing Checklist

Use this checklist to verify that the opencode-tutor plugin is working correctly after installation or modification.

## Setup

1. Install the plugin (see [README.md](README.md) for installation instructions).
2. Open opencode in a project directory.

## Verification steps

### Switch to tutor agent

- [ ] Run `/agent tutor` in opencode
- [ ] Confirm the active agent changes to `tutor`

### Onboarding

- [ ] Send a message like: `I want to learn Python async/await. I'm a beginner and prefer hints over full solutions.`
- [ ] Confirm the tutor responds with onboarding questions or acknowledges your input
- [ ] Confirm `~/.opencode/tutor/learner-profile.md` is created with the correct structure (Snapshot, Learning goals, Primary topics, Strengths, Sticking points, Notes sections)

### Track creation

- [ ] Send a message like: `I want to learn Redis caching patterns`
- [ ] Confirm `~/.opencode/tutor/tracks/redis-caching/` (or similar slug) is created
- [ ] Confirm all four files exist: `track.md`, `project.md`, `roadmap.md`, `progress.md`
- [ ] Confirm `roadmap.md` contains `- [ ]` checkbox items
- [ ] Confirm `progress.md` contains a `## Journey status` section

### Track resumption

- [ ] Start a new session (or clear context)
- [ ] Send: `I want to keep learning Redis caching`
- [ ] Confirm the tutor reads the existing track files and continues from the saved state (current focus / next step)

### Hint ladder

- [ ] Ask for help with a specific implementation task (e.g., `how do I set a TTL on a Redis key?`)
- [ ] Confirm the first response is a nudge or directional hint, not a full solution
- [ ] Ask again or indicate you're stuck — confirm the hint escalates one level
- [ ] Explicitly ask for the full solution — confirm the tutor provides it

### Reflection and progress update

- [ ] Send a reflection: `I finished the basic SET/GET exercise but I'm still unsure about key expiry`
- [ ] Confirm `progress.md` is updated with: the reflection in `## Reflections`, a blocker if applicable, a refreshed `## Next step`, and an updated timestamp

### Roadmap checkbox sync

- [ ] Complete a roadmap task and report it to the tutor
- [ ] Confirm the relevant `- [ ]` in `roadmap.md` is changed to `- [x]`
- [ ] Confirm `## Journey status` count in `progress.md` is updated to match

### Next step

- [ ] Ask: `what should I do next for Redis caching?`
- [ ] Confirm the tutor reads roadmap and progress, then returns one concrete next step
- [ ] Confirm `progress.md` is updated if the next step changed

### Switch away from tutor mode

- [ ] Run `/agent code`
- [ ] Confirm the active agent changes back to the default coding agent
- [ ] Confirm tutor-specific behaviors (hint-first, onboarding check) are no longer active
