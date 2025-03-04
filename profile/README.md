# COLAV (Artemis-QUB) 

Our research develops an autonomous marine navigation system that ensures safe, efficient operation in dynamic environments while maintaining real-time compliance with COLREGs. Our motion planner prioritizes provable safety, balancing simplicity with competitiveness. Unlike modern deep-learning "black box" methods, it remains interpretable yet more advanced than basic state machines methods, enabling effective handling of complex scenarios.
We are designing COLAV as a plug-and-play module for any vessel with proper integration. In collaboration with Artemis, experts in sustainable hydrofoil technology, we leverage hydro foiling's simpler dynamics to refine and validate our system in both simulations and real-world tests.

## Hybrid Automaton

The Hybrid Automaton is the core of our COLAV system, responsible for real-time trajectory planning and generating velocity-yaw rate commands at each transition. It operates in five control modes which have distinct functions:
- Cruise Mode: Maintains a zero-yaw rate when on a direct line of sight to the target waypoint/virtual waypoint Transitioning  to Waypoint Mode upon reaching the waypoint acceptance radius, completing motion planning.
- Fallback Mode: Engages when an unexpected scenario occurs triggering evasive maneuvers or some other functionality.
- T2LOS Mode: Activates when the vessel is off-course from the virtual waypoint/goal waypoint, outputs yaw rate commands to vessel until vessel reaches zero error rate with line of sight.
- T2Theta Mode: Triggers if the trajectory is predicted to intersect the unsafe set within a threshold distance. It generates a new virtual waypoint, transitions to T2LOS for course correction, and then returns to Cruise Mode, repeating until the final waypoint is reached.
Waypoint Mode: Indicates successful arrival within the acceptance radius closing the hybrid automaton.

[!image](./images/hybrid_automaton.png)

## Architecture

Hybrid automata integration in real-time systems is challenging. Our ROS-based COLAV server enables concurrent execution of key modules:

- [colav_gateway](): Manages mission requests, updates, and feedback via Prototoc Buffers enabling seamless conversion between ROS interfaces published through network communciation
- [colav_risk_assessment](): Computers real time unsafe sets, predicts trajectories and assesses collision risk for adaptive collision avoidance.
- [colav_mapping](): Processes OSM and sensor data to create static/dynamic map layers. It combines rasterized vector maps with unsafe set overlays, applyinjg bloating for cost map generation.
- [colav_hybrid_automaton](): Implements state-switching logic with Simulink Stateflow and ROS, generating yaw-rate/velocity commands while managing system transitions in real time.

This architecture ensures safe, adaptive navigation with real-time vessel sensor integration.
[!image](./images/colav%20archicture.png)

## Testing

Our COLAV system is designed to be easily integrable via Protobuf I/O, enabling seamless testing. We developed a geometric simulator that runs real-world marine scenarios, including dynamic obstacles with state, trajectory, and geometry data, as well as static obstacles like landmasses and harbours. Using commonocean-io, we stream real-time mock sensor fusion data and retrieve velocity and yaw rate commands from the hybrid automaton. The drivability checker ensures feasible trajectories, while commonocean-rules enforces COLREG compliance. This allows systematic validation of the system’s decision-making and trajectory generation. Looking ahead, we plan to test in 3D game engine simulations with hydrofoils for Artemis and ultimately deploy the system on a real vessel.

[!image](./images/colav_sim_vessel_integration.png)