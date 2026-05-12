# 07 - Beyond Behaviour Trees: GOAP and Utility AI

You have now built a Behaviour Tree from scratch, added perception, and implemented multi-agent coordination or be close to completing it. This session steps back from implementation and looks at two fundamentally different approaches to game AI: Goal-Oriented Action Planning (GOAP) and Utility AI. Understanding why these systems were invented makes you a better Behaviour Tree designer, and prepares you for the AI systems you will encounter in industry.

---

## The Behaviour Tree's Limitations

Before looking at alternatives, it is worth naming what Behaviour Trees are genuinely bad at.

**Fixed priority ordering.** The leftmost branch always has the highest priority. This is excellent when the ordering is stable, but becomes a problem when priority should shift dynamically. A guard that should always attack when it can, except when outnumbered, except when low on health, except when the player is in cover: encoding all those exceptions in a fixed-priority tree produces a deeply nested, brittle structure.

**Manual transition encoding.** Every possible interruption requires a Decorator configured correctly. As the number of behaviours grows, the number of potential interactions between them grows faster. You end up reasoning about every combination.

**Duplication across agent types.** A different agent with similar but not identical behaviour means duplicating large parts of the tree and modifying it. There is no natural mechanism for sharing logic between tree variants.

These limitations are why the industry invented alternatives.

---

## GOAP: Goal-Oriented Action Planning

### Where it came from

GOAP was introduced to mainstream game development by the 2005 game FEAR. The enemies in FEAR were notable for their time: they flanked, used cover, communicated with each other, and adapted to the player's tactics. The AI programmer Jeff Orkin published the technique, and it has been studied and used ever since.

### How it works

In GOAP, the agent does not have a tree of behaviours. Instead it has:

- A **goal**: a desired world state (PlayerAlive = false)
- A set of **actions**, each with preconditions and effects
- A **planner** that searches for a sequence of actions leading from the current world state to the goal

The planner is essentially A* on a graph of world states. The agent defines what it wants to achieve; the planner figures out how to achieve it given the available actions and current world conditions.

```
Current state:  PlayerAlive=true, InCover=false, HasAmmo=true
Goal:           PlayerAlive=false

Actions:
  Shoot         precondition: HasLineOfSight=true    effect: PlayerAlive=false
  MoveToLOS     precondition: InCover=false           effect: HasLineOfSight=true
  Reload        precondition: HasAmmo=false           effect: HasAmmo=true

Planner finds:  MoveToLOS -> Shoot
```

If the player moves mid-execution, the planner re-runs and finds a new sequence. The agent adapts automatically without the designer anticipating every scenario.

### Why it is powerful

Adding a new action: Reload, for example, automatically makes it available to any plan that needs it. A guard who runs out of ammo will plan: reload, then move to line of sight, then shoot. The designer did not need to find every place in the tree where reloading would be relevant and add a branch for it.

FEAR's enemies felt intelligent because they were solving problems, not following scripts.

### The tradeoffs

- **Planning cost.** A* on a world state graph is expensive if the action space is large. FEAR kept actions to 10-15 to maintain performance.
- **Brittle mid-plan.** If a precondition becomes invalid while an action is executing, the agent re-plans. This can cause hesitation or erratic direction changes at the seam.
- **Hard to author.** A Behaviour Tree can be read and modified by a designer who has never written code. A GOAP action graph requires understanding the planner to debug unexpected behaviour.
- **World state modelling.** Every relevant fact must be reduced to a boolean or simple value. Rich, continuous game state is hard to compress into this format.

### Where you see it today

Tomb Raider (the 2013 reboot), some Far Cry titles, and several indie games use GOAP variants. It is not the dominant approach in the industry but it appears regularly in games where emergent NPC problem-solving is a design goal.

---

## Utility AI

### How it works

Utility AI removes both fixed priority and planning. Instead, every possible action has a **utility function**: a formula that scores how desirable that action is right now based on current game state. On every evaluation, the agent scores all actions and takes the highest-scoring one.

| Action | Utility formula | Score |
|---|---|---|
| Eat | Hunger level | 80 |
| Sleep | Fatigue level | 20 |
| Socialise | Loneliness level | 45 |
| Work | Financial need | 30 |

This is The Sims. The agent always does the most important thing right now. No transitions. No priority ordering. Behaviour emerges from the scores.

### Why it is powerful

The same agent produces genuinely different behaviour in different situations because the scores change continuously. There is no fixed priority to fight against. Behaviours blend naturally: the agent does not rigidly finish one task before considering another, it re-evaluates constantly.

It also handles partial satisfaction. A Behaviour Tree commits to one branch until a Decorator fires. A Utility system might eat a little, then immediately re-evaluate and sleep if energy dropped enough in the meantime.

### The tradeoffs

- **Debugging is hard.** When an agent does something unexpected you have to examine every utility function to find which score dominated. The Behaviour Tree debugger shows you exactly which node ran and why. A utility debugger shows you a list of scores.
- **Tuning is art.** Getting utility curves to produce natural-feeling behaviour requires iteration. Small changes in one function can produce large behaviour differences.
- **Designer legibility.** "The agent chases when CanSeePlayer is true" is understandable to a non-programmer. "The agent chases because Chase scored 0.73 versus Patrol's 0.41" is less understandable.
- **Not common in action games.** Utility AI works best when many behaviours compete with roughly equal importance. Action and stealth games usually have clear fixed priorities (attack beats patrol, always) which makes Behaviour Trees a natural fit.

### Where you see it today

The Sims is the canonical example. Spore used a simplified version. Some strategy games use it for economic and unit management. It also appears in dialogue systems where an NPC evaluates possible conversation topics and picks the most contextually relevant one.

---

## Choosing the Right Tool

| Approach | Use when |
|---|---|
| Behaviour Tree | Priority ordering is stable, designers need to author behaviour, debugging must be step-by-step |
| GOAP | AI needs to solve open-ended problems, action sequences are not known at design time |
| Utility AI | Many behaviours compete with roughly equal importance, adaptivity matters more than predictability |

Most shipped games use Behaviour Trees. Not because they are theoretically best, but because they are tooled, understood, debuggable, and good enough. GOAP and Utility AI appear in niches where their specific strengths are genuinely needed.

The most valuable thing this comparison gives you is a vocabulary for recognising when you are fighting your Behaviour Tree. When a tree gets deeply nested, when Decorator configurations become hard to reason about, when the priority ordering keeps needing exceptions: those are signals that the problem might fit GOAP or Utility AI better.
