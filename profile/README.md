# COLAV (Artemis-QUB) 

Our research develops an autonomous marine navigation system that ensures safe, efficient operation in dynamic environments while maintaining real-time compliance with COLREGs. Our motion planner prioritizes provable safety, balancing simplicity with competitiveness. Unlike modern deep-learning "black box" methods, it remains interpretable yet more advanced than basic state machines methods, enabling effective handling of complex scenarios.
We are designing COLAV as a plug-and-play module for any vessel with proper integration. In collaboration with Artemis, experts in sustainable hydrofoil technology, we leverage hydro foiling's simpler dynamics to refine and validate our system in both simulations and real-world tests.

The Hybrid Automaton is the core of our COLAV system, responsible for real-time trajectory planning and generating velocity-yaw rate commands at each transition. It operates in five control modes which have distinct functions:
- Cruise Mode: Maintains a zero-yaw rate when on a direct line of sight to the target waypoint/virtual waypoint Transitioning  to Waypoint Mode upon reaching the waypoint acceptance radius, completing motion planning.
- Fallback Mode: Engages when an unexpected scenario occurs triggering evasive maneuvers or some other functionality.
- T2LOS Mode: Activates when the vessel is off-course from the virtual waypoint/goal waypoint, outputs yaw rate commands to vessel until vessel reaches zero error rate with line of sight.
- T2Theta Mode: Triggers if the trajectory is predicted to intersect the unsafe set within a threshold distance. It generates a new virtual waypoint, transitions to T2LOS for course correction, and then returns to Cruise Mode, repeating until the final waypoint is reached.
Waypoint Mode: Indicates successful arrival within the acceptance radius closing the hybrid automaton.
