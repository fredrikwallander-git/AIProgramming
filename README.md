# AIProgramming

Course material for the AI Programming section at Forsbergs skola.

Assignment information and requirements:

G (Pass)<br>
All of the following are present and working:

* A NavMesh covers the walkable area of the level
* At least one agent patrols the level autonomously using a Behaviour Tree
* A player-controlled character is in the scene
* Exactly one AIPerception sense (sight or hearing, student's choice) produces at least two clearly distinct agent states: patrol before detection and a visible reaction after detection. Both states must be demonstrable in the submitted video with the perception sense clearly triggering the transition.
* The agent returns to patrol after losing detection. An agent that remains in a chase or alert state permanently after the player hides does not satisfy this requirement.
* GitHub repository with Git LFS tracking, a recorded video demonstrating the AI, and a README that explains the Behaviour Tree structure, the Blackboard keys used and their purpose, and what the chosen perception sense triggers. A surface-level project description without implementation detail does not satisfy the README requirement.


VG (Excellent)<br>
All G requirements are met.

* The second perception sense: whichever sense was not used for G must now be added. Both sight and hearing must be present in the project and each must produce a clearly different agent response. The two senses do not need to be on the same agent: one agent type may use sight and another hearing, but both must lead to visibly distinct behaviours.
* Team alertness: when one agent detects the player, other agents in the scene must visibly react by changing state: moving to investigate, switching out of patrol, or converging on the last known player position. An agent that continues its normal patrol routine while a nearby agent is actively chasing or investigating does not satisfy this requirement.