R■HIRI 2.0
Cyber-Organic Visual Being System
Complete Edition — Technical Documentation
An interactive artificial life simulation featuring a responsive particle swarm entity that perceives and
reacts to user presence. Built with pure Python using NumPy for computation and Matplotlib for
real-time visualization.
Key Features:
• Real-time particle physics with Boids flocking algorithm
• Dynamic emotional system with 5 distinct emotions
• Resonance-based user-entity connection mechanic
• Procedural narrative generation
• Visible user avatar for tangible interaction
• Zero external dependencies beyond NumPy/Matplotlib
Version 2.0 Complete Edition
Author: Claude (Anthropic)
Generated: January 08, 2026
Table of Contents
1. Introduction ........................................ 3
1.1 Design Philosophy .............................. 3
1.2 Technical Requirements ......................... 3
2. System Architecture ................................. 4
2.1 Component Overview ............................. 4
2.2 Data Flow ...................................... 5
3. Configuration System ................................ 6
3.1 World Configuration (Config) ................... 6
3.2 Genetic Traits (DNA) ........................... 7
4. Core Systems ........................................ 8
4.1 User Avatar .................................... 8
4.2 Particle Swarm ................................. 9
4.3 Emotional Core ................................. 10
4.4 Consciousness Layer ............................ 11
4.5 Energy System .................................. 12
4.6 Resonance System ............................... 12
5. Physics Engine ...................................... 13
5.1 Flocking Algorithm ............................. 13
5.2 Force Integration .............................. 14
5.3 Boundary Handling .............................. 14
6. Visualization ....................................... 15
6.1 Display Layout ................................. 15
6.2 Visual Elements ................................ 16
6.3 Animation Loop ................................. 17
7. API Reference ....................................... 18
8. Usage Guide ......................................... 19
9. Future Enhancements ................................. 20
1. Introduction
R■HIRI 2.0 is an interactive artificial life simulation that creates a mesmerizing experience of guiding a
digital entity through a virtual space. The system combines principles from swarm intelligence, affective
computing, and real-time graphics to produce an entity that feels alive and responsive.
1.1 Design Philosophy
The complete edition was designed with several core principles in mind:
• Tangible Presence: Unlike the original where user input was invisible, this version gives users a
visible avatar (cyan diamond) so they can see themselves in the digital space.
• Emergent Behavior: Complex behaviors emerge from simple rules rather than being explicitly
programmed.
• Emotional Authenticity: The entity's emotional responses follow plausible dynamics with
momentum and decay.
• Minimal Dependencies: Uses only NumPy and Matplotlib, making it easy to run anywhere
Python exists.
• Clean Architecture: Each system is self-contained with clear interfaces, making the code
maintainable and extensible.
1.2 Technical Requirements
Component Requirement
Python 3.8 or higher
NumPy 1.20 or higher
Matplotlib 3.5 or higher
RAM ~100 MB
Display Minimum 1280×720, GUI support required
Table 1: System Requirements
Installation:
pip install numpy matplotlib
Running:
python RAHIRI_2_Complete_Game.py
2. System Architecture
The system follows a modular architecture where each component handles a specific aspect of the
simulation. The RahiriEngine class orchestrates all subsystems, while the Visualizer handles rendering
and input.
2.1 Component Overview
Component Responsibility State
Config System parameters Immutable
DNA Personality traits Mostly immutable
UserAvatar Player representation Position, velocity, trail
ParticleSwarm Entity particles Positions, velocities, energies
EmotionalCore Emotional dynamics 5 emotion values + momentum
ConsciousnessLayer Narrative generation Cooldown, history
EnergySystem Vitality management Vitality, stamina
ResonanceSystem User-entity bond Resonance level
RahiriEngine Orchestration Time, frame count
Visualizer Rendering & input All visual elements
Table 2: System Components
2.2 Data Flow
Each frame, data flows through the system in a defined sequence:
1. Input Capture: Arrow key states are read from the Visualizer's key tracking dictionary.
2. User Update: UserAvatar converts input to acceleration, updates velocity with damping, and
moves position with world wrapping.
3. Resonance Calculation: Distance between user and swarm center determines resonance
growth/decay.
4. Energy Update: Combined activity level (user + swarm) determines energy consumption and
regeneration.
5. Swarm Physics: Flocking forces (cohesion, alignment, separation) plus user attraction are
integrated.
6. Emotional Evolution: Emotions update based on user proximity, activity, and swarm
coherence.
7. Narrative Selection: ConsciousnessLayer picks contextually appropriate text based on
emotional state.
8. Rendering: Visualizer updates all graphical elements with new state data.
The simulation runs at approximately 30 FPS (33ms frame interval). Each step() call advances
simulation time by 0.033 seconds. The visualization uses Matplotlib's FuncAnimation for smooth
updates.
3. Configuration System
The simulation is controlled by two dataclasses: Config for world/physics parameters and DNA for
personality traits. Both use Python's @dataclass decorator for clean initialization with sensible defaults.
3.1 World Configuration (Config)
Parameter Default Description
world_width 100.0 Virtual world width in units
world_height 100.0 Virtual world height in units
n_particles 50 Number of swarm particles
particle_size_base 60.0 Base particle display size
particle_size_variance 40.0 Size variation range
max_particle_speed 1.8 Maximum particle velocity
max_user_speed 2.5 Maximum user avatar velocity
velocity_damping 0.92 Velocity retention per frame
cohesion_weight 0.012 Flocking cohesion strength
alignment_weight 0.008 Flocking alignment strength
separation_weight 0.025 Flocking separation strength
separation_radius 8.0 Personal space radius
user_attraction_weight 0.15 User influence strength
user_attraction_radius 50.0 User influence range
resonance_growth_rate 0.02 Resonance increase rate
resonance_decay_rate 0.005 Resonance decrease rate
connection_max_distance 20.0 Max distance for visual connections
trail_length 15 User trail history frames
frame_interval 33 Milliseconds between frames
Table 3: Configuration Parameters
3.2 Genetic Traits (DNA)
DNA traits define the entity's personality. These values influence behavior across multiple systems,
creating consistent character traits.
Trait Default Affects
curiosity 0.8 Curiosity emotion sensitivity, exploration tendency
sensitivity 0.85 Overall responsiveness to stimuli
creativity 0.7 Behavioral variation, metabolic rate
sociability 0.6 Group cohesion preference (reserved for future)
energy_base 0.9 Starting vitality level
Table 4: DNA Traits
How DNA affects behavior:
• curiosity directly scales the curiosity emotion's stimulus response
• sensitivity amplifies joy response to user proximity
• creativity modifies the metabolism rate in EnergySystem
• energy_base sets initial vitality (affects swarm responsiveness)
4. Core Systems
4.1 User Avatar
The UserAvatar class represents the player in the virtual world. Unlike the original implementation
where user input was invisible, this version renders the user as a cyan diamond shape with a glowing
aura and trailing effect.
State Variables:
• position: numpy array [x, y] — current world coordinates
• velocity: numpy array [x, y] — current movement vector
• acceleration: numpy array [x, y] — force accumulator
• trail: deque of past positions (15 frames)
• glow_phase: float — animation phase for pulsing effect
• input_direction: numpy array [x, y] — raw input
Movement Physics:
The avatar uses a simple but responsive physics model:
acceleration = input_direction × 0.3
velocity = velocity × 0.85 + acceleration
velocity = clamp(velocity, max_user_speed)
position = (position + velocity) mod world_size
The 0.85 damping factor creates smooth deceleration when keys are released, while the modulo
operation creates a toroidal (wrapping) world.
4.2 Particle Swarm
The ParticleSwarm is the heart of R■HIRI — a collection of 50 particles that move as a cohesive entity
while responding to user presence. The implementation uses vectorized NumPy operations for
efficiency.
Initialization Pattern
Particles are initialized in two groups for visual interest:
• Core cluster (50%): Arranged in a circle of radius 8-10 around center
• Satellites (50%): Scattered in a ring of radius 15-20 around center
This creates an organic "nucleus and electrons" appearance at startup.
Per-Particle Properties
Property Type Purpose
positions ndarray[n, 2] World coordinates
velocities ndarray[n, 2] Movement vectors
energies ndarray[n] Individual energy (affects size/brightness)
phases ndarray[n] Animation phase for pulsing effect
Table 5: Particle Properties
Coherence Metric
Swarm coherence measures how tightly grouped the particles are:
distances = ||positions - center_of_mass||
coherence = clamp(1 - mean(distances) / 30, 0, 1)
High coherence (close to 1) indicates a tight cluster; low coherence indicates dispersal. This metric
influences emotional responses and narrative selection.
4.3 Emotional Core
The EmotionalCore manages five distinct emotions, each with individual dynamics. Unlike simple state
machines, emotions here have momentum for smooth transitions and decay rates for natural
diminishment.
Emotion Initial Decay Stimulus Source Color
Joy 0.5 0.995 User proximity #FFD700 (Gold)
Curiosity 0.7 0.990 User activity level #00CED1 (Teal)
Excitement 0.3 0.985 Activity + proximity #FF6B6B (Coral)
Calm 0.6 0.998 Low activity × coherence #98FB98 (Mint)
Wonder 0.4 0.992 Proximity × activity #DDA0DD (Lavender)
Table 6: Emotion Parameters
Update Algorithm:
For each emotion per frame:
1. Apply decay: value *= decay_rate
2. Calculate stimulus based on context
3. Update momentum: momentum = 0.9×momentum + 0.1×stimulus
4. Apply momentum: value = clamp(value + momentum, 0.05, 1.0)
The momentum smoothing prevents jarring emotional shifts.
4.4 Consciousness Layer
The ConsciousnessLayer generates contextual narrative text that gives voice to the entity's
experience. It selects from themed narrative banks based on the current emotional and resonance
state.
Narrative Themes:
• greeting: Used in first 2 seconds ("I sense your presence...", "Welcome to my realm.")
• connection: High resonance > 0.7 ("We move as one.", "Our paths intertwine.")
• wonder: Dominant curiosity ("What mysteries do you bring?")
• joy: Dominant joy ("This brings me joy.", "Harmony flows through us.")
• excitement: Dominant excitement ("Energy surges!", "The dance intensifies!")
• calm: Dominant calm ("Stillness has beauty.", "Tranquil moments.")
Selection Logic:
1. Check for special conditions (time < 2s → greeting, resonance > 0.7 → connection)
2. Otherwise, map dominant emotion to theme
3. Select random narrative from theme, avoiding immediate repetition
4. Set cooldown of 90-180 frames (3-6 seconds) before next change
4.5 Energy System
The EnergySystem tracks vitality (overall energy) and stamina (burst capacity). Energy affects swarm
responsiveness — low vitality means slower, smaller movements.
Energy Dynamics:
cost = activity_level × 0.0003 × metabolism
regen = 0.0005 × (1 + (1 - activity_level) × 0.5)
vitality = clamp(vitality - cost + regen, 0.3, 1.0)
Vitality never drops below 30%, ensuring the entity remains somewhat responsive even after
prolonged high activity.
4.6 Resonance System
Resonance measures the strength of the connection between user and entity. It's the key mechanic
that rewards sustained, close interaction.
Resonance Mechanics:
• Growth condition: User is active AND proximity > 0.3
• Growth rate: proximity × 0.02 per frame
• Decay rate: 0.005 per frame when conditions not met
• Range: 0.0 (disconnected) to 1.0 (fully synchronized)
Effects of high resonance:
• Swarm is more strongly attracted to user
• Connection beam becomes visible (> 30%)
• Particle connection distance increases
• Unlocks "connection" narrative theme (> 70%)
5. Physics Engine
The physics simulation combines the classic Boids algorithm with user interaction forces. All
calculations are vectorized using NumPy for efficiency.
5.1 Flocking Algorithm (Boids)
The Boids algorithm simulates flocking behavior through three simple rules that, when combined,
produce complex emergent behavior.
Rule Formula Weight
Cohesion center_of_mass - position 0.012
Alignment mean_velocity - velocity 0.008
Separation -Σ(offset / distance) for nearby 0.025
Table 7: Flocking Forces
Cohesion: Each particle is attracted toward the swarm's center of mass, keeping the group together.
Alignment: Each particle adjusts its velocity toward the swarm's average velocity, creating
coordinated movement.
Separation: Particles repel neighbors within 8 units, preventing collisions and creating natural spacing.
The repulsion strength is inversely proportional to distance.
5.2 Force Integration
All forces are combined and integrated using semi-implicit Euler:
total_force = (cohesion × 0.012 + alignment × 0.008 +
separation × 0.025 + user_attraction × 0.15) × vitality
velocity = velocity × 0.92 + total_force
velocity = clamp(velocity, max_particle_speed × vitality)
position = position + velocity
The vitality scaling means a tired entity moves more sluggishly.
5.3 Boundary Handling
The world uses soft boundaries with a hard clamp backup. When particles approach within 5 units of
an edge, a repulsive force pushes them back.
Boundary Forces:
if position.x < 5: velocity.x += 0.5
if position.x > width - 5: velocity.x -= 0.5
if position.y < 5: velocity.y += 0.5
if position.y > height - 5: velocity.y -= 0.5
# Hard clamp as backup
position = clamp(position, 1, world_size - 1)
Note: The user avatar uses different boundary handling — it wraps around (toroidal topology),
allowing movement through edges to appear on the opposite side.
6. Visualization
The Visualizer class handles all rendering using Matplotlib. The display is divided into four panels
providing different views of the system state.
6.1 Display Layout
Panel Position Size Content
Main View Left 65% × 80% Particles, user, connections
Emotions Top-right 24% × 40% 5 horizontal emotion bars
Resonance Bottom-right 24% × 35% Time series + percentage
Narrative Bottom 65% × 10% Current narrative text
Table 8: Panel Layout
6.2 Visual Elements
Main View Elements
• Particles: Scatter plot with 'plasma' colormap. Size varies with energy and animation phase (60-100
pixels). Color intensity reflects energy × resonance.
• Connections: LineCollection drawing white lines between particles within threshold distance. Alpha
fades with distance for depth effect.
• User Avatar: Diamond marker (■) in cyan (#00FFFF) with white edge. Size 200 pixels.
• User Glow: Large circular marker behind avatar. Size pulses with sin(phase) and scales with
resonance. Alpha 0.1-0.25.
• User Trail: Line plot of last 15 positions. Alpha scales with resonance.
• Connection Beam: Dashed line from user to swarm center. Only visible when resonance > 30%.
Alpha = resonance × 0.3.
Side Panel Elements
• Emotion Bars: Five horizontal bars colored to match each emotion. Width represents current value
(0-1).
• Resonance Graph: Time series of last 100 resonance values. Filled area under curve in cyan with
20% opacity. Large percentage text in top-right.
• Narrative Text: Centered italic text. Color changes to match dominant emotion's color.
6.3 Animation Loop
The animation uses Matplotlib's FuncAnimation with a 33ms interval (~30 FPS). Each frame executes
the following sequence:
1. Read keyboard state from self.keys_pressed dictionary
2. Convert to direction vector via _get_input_vector()
3. Pass input to engine via set_user_input(dx, dy)
4. Execute simulation step via engine.step()
5. Update particle scatter (positions, sizes, colors)
6. Update connection lines between nearby particles
7. Update user avatar, glow, and trail
8. Update emotion bar widths
9. Update resonance line and fill
10. Update narrative text and color
11. Update statistics text
12. Return list of modified artists
Input Handling:
Keyboard events are captured via mpl_connect. Key states are stored in a dictionary and converted to
a direction vector each frame:
keys_pressed = {'up': False, 'down': False, 'left': False, 'right': False}
def _get_input_vector():
dx = (1 if right else 0) - (1 if left else 0)
dy = (1 if up else 0) - (1 if down else 0)
return dx, dy
7. API Reference
This section documents the public interfaces of the main classes for developers who wish to extend or
modify the system.
RahiriEngine
Constructor: RahiriEngine()
Creates engine with default Config and DNA. Initializes all subsystems.
Methods:
• set_user_input(dx: float, dy: float)
Sets user movement direction. Values typically -1, 0, or 1.
• step() → Tuple[Dict, Dict]
Executes one simulation frame. Returns (particle_data, system_data).
Attributes:
• config: Config instance
• dna: DNA instance
• user: UserAvatar instance
• swarm: ParticleSwarm instance
• time: float — simulation time in seconds
• frame: int — frame counter
Return Data Structures
particle_data dictionary:
{
'positions': ndarray[n, 2], # World coordinates
'velocities': ndarray[n, 2], # Movement vectors
'speeds': ndarray[n], # Velocity magnitudes
'energies': ndarray[n], # Per-particle energy
'phases': ndarray[n], # Animation phases
'center': ndarray[2], # Center of mass
system_data dictionary:
}
{
'user_position': ndarray[2],
'user_velocity': ndarray[2],
'user_trail': List[ndarray],
'resonance': float, # 0.0 to 1.0
'emotions': Dict[str, float], # 5 emotion values
}
'dominant_emotion': Tuple[str, float],
'energy': Dict[str, float], 'narrative': str,
'coherence': float, # 0.0 to 1.0
'time': float,
'frame': int,
# vitality, stamina, metabolism
8. Usage Guide
Getting Started
1. Launch the simulation:
python RAHIRI_2_Complete_Game.py
2. Wait for initialization:
The console will display dependency checks and initialization messages.
3. Interact:
Use arrow keys to move your cyan diamond avatar. The particle swarm will respond to your presence.
Controls
Key Action
↑ (Up Arrow) Move avatar upward
↓ (Down Arrow) Move avatar downward
← (Left Arrow) Move avatar left
→ (Right Arrow) Move avatar right
ESC Exit simulation
Table 9: Keyboard Controls
Tips for Interaction
• Build resonance: Stay close to the swarm center while moving. The resonance meter
(bottom-right) will fill up.
• Watch the emotions: Different behaviors trigger different emotions. Fast movement increases
excitement; staying still promotes calm.
• Follow the narrative: The text at the bottom reflects the entity's experience. High resonance
unlocks special connection messages.
• Lead the swarm: Once resonance is high, try leading the swarm around the world. It will follow
you more eagerly.
• Experiment with pacing: Alternate between active movement and stillness to observe different
emotional responses.
9. Future Enhancements
While R■HIRI 2.0 Complete Edition is fully functional, several enhancements could extend its
capabilities and engagement potential.
Audio Integration: Add procedural audio that responds to emotional state and activity. Sonify the
particle movements, resonance level, and emotional transitions using a library like PyGame or
sounddevice.
Persistent Evolution: Save and load entity state between sessions. Track cumulative experience
and allow DNA traits to evolve over many play sessions.
Multiple Entities: Introduce additional R■HIRI entities that can interact with each other, creating
emergent social dynamics.
Environmental Features: Add obstacles, attractors, or zones with special properties that affect
particle behavior.
Extended Narrative: Implement a more sophisticated narrative system with memory of past
interactions and evolving relationship depth.
GPU Acceleration: Port physics calculations to CUDA for significantly larger particle counts
(1000+).
Network Mode: Allow multiple users to connect and interact with the same entity simultaneously.
VR/AR Support: Adapt the visualization for immersive displays using libraries like OpenGL or
Unity integration.
R■HIRI 2.0 demonstrates that meaningful interactive experiences can emerge from relatively simple
rules when thoughtfully combined. The entity's behaviors, while deterministic at the code level, feel
organic and responsive because they arise from the interplay of multiple systems rather than being
explicitly scripted.
— End of Document —
Generated 2026-01-08 17:21 | R■HIRI 2.0 Complete Edition
