# 09 - Submission

This is the final session. No new content. The entire time is for finishing, polishing, and later submitting your assignment.

---

## Submission Checklist

Work through this list to reach the goals for the assignment. Everything here is required.

### Required for all students (G minimum)

- [ ] GitHub repository exists and is pushed with recent commits
- [ ] `git lfs ls-files` in the terminal lists `.uasset` and `.umap` files, if not, LFS is not tracking binaries correctly
- [ ] A NavMesh covers the walkable area of the level
- [ ] At least one agent patrols the level autonomously using a Behaviour Tree
- [ ] A player-controlled character is in the scene
- [ ] One AIPerception sense (sight or hearing) produces at least two clearly distinct agent states: patrol before detection and a visible reaction after. Both must be demonstrable in the video.
- [ ] The agent visibly returns to patrol after losing detection: it does not chase indefinitely or freeze
- [ ] README in the repository root (see README requirements below)
- [ ] Recorded video demonstrating the AI

### Additional for VG

- [ ] The second perception sense (whichever was not used for G) is implemented and produces a clearly different agent response from the first sense
- [ ] Team alertness: when one agent detects the player, others visibly react by changing state: an agent continuing normal patrol while a neighbour is chasing does not satisfy this
- [ ] README covers all VG items (see below)

---

## README Requirements

The README is your written report. It is assessed alongside the video. A surface-level description of the project does not satisfy the requirement.

### Minimum content for G

- What the project is: one paragraph covering the game genre, theme, and what the AI agents are supposed to represent
- How the Behaviour Tree is structured: describe the branches (patrol, chase or other reaction), what each one does, and what triggers the transitions between them
- What Blackboard keys are used, what type each one is, and why it exists
- Which perception sense was implemented, what it detects, and what state change it triggers in the agent
- One thing you would add or change with more time

### Additional content for VG

- Explanation of how the second sense produces a different response from the first: what it detects, what Blackboard keys it writes, and what Behaviour Tree branch it activates
- How the team alertness system was implemented and what approach was used (Game Instance, broadcast, shared Blackboard)
- Reflection: what worked well, what did not, what you would add

Write it as if explaining the project to another programmer who has not seen your code. Be specific. Do not say "I implemented AI behaviour", say "the guard has three Behaviour Tree branches: Chase (triggered by sight, uses IsChasing as the Decorator key), Investigate (triggered by hearing, moves to NoiseLocation), and Patrol (default, uses per-instance Target Points set in the Details panel)."

---

## Video Requirements

The video must show all features you are claiming in your submission. Do not assume the reviewer will infer anything from watching.

### For G

- Show the agent patrolling autonomously
- Show the player triggering a visible state change through the implemented sense
- Show the agent returning to patrol after the player hides or moves out of range

### Additional for VG

- Show each sense producing a different response: demonstrate them separately and clearly. Trigger sight first, then trigger hearing from a different position. The agent response must be visibly different for each.
- Show team alertness firing: walk in front of one guard and show another guard visibly changing state in the same frame. The reacting guard must not continue its normal patrol routine.

A nice addition to the video is to add text to infer what is happening, or use your microphone to capture what you are triggering and what the expected response from the guard would be.

Raw screen capture is acceptable. No editing required. OBS is free.

---

## Git Final Check

Before submitting, run these commands in a terminal from your project root:

```bash
git status
```
This should show no uncommitted changes after your final commit.

```bash
git lfs ls-files
```
This should list `.uasset` and `.umap` files. If nothing appears, LFS is not tracking your binaries and large files may not have pushed correctly.

```bash
git log --oneline -5
```
This shows your five most recent commits. Verify your latest work is there.

Final commit and push:

```bash
git add .
git commit -m "Final submission"
git push
```

Verify on GitHub that the push succeeded, the README is visible on the repository front page, and the video is either linked in the README or attached to the repository.
