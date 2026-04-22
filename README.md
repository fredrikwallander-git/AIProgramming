# AIProgramming

Course material for the AI Programming section at Forsbergs skola.


G (Pass)
All of the following are present and working:

A NavMesh covers the walkable area of the level
At least one agent patrols the level autonomously
A player-controlled character is in the scene
Exactly one AIPerception sense (sight or hearing, student's choice) causes the agent to behave differently when the player is detected versus when the player is absent — this must be visible in the submitted video
GitHub repository with Git LFS tracking, a README describing the project, and a recorded video demonstrating the AI

VG (Excellent)
All G requirements are met.
The second perception sense: whichever sense was not used for G must now be added. Both sight and hearing must be present in the project and each must produce a clearly different agent response. The two senses do not need to be on the same agent — one agent type may use sight and another hearing — but both must lead to visibly distinct behaviours.
Team alertness: when one agent detects the player, other agents in the scene must visibly react by changing state — moving to investigate, switching out of patrol, or converging on the last known player position. An agent that continues its normal patrol routine while a nearby agent is actively chasing or investigating does not satisfy this requirement.