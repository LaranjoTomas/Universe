---
tags:
  - "#TPJ"
---
## History of Game Programming (P2)
• **The Early Era**: The first digital game is attributed to **Edward Condon (1940)**, who built an electromechanical machine to play _Nim_. The **"Brown Box" (1967)** was the first domestic device, followed by the popularization of home consoles by **Atari**
• **Mainframes and Arcades**: Early mainframe games like _Oregon Trail_ (1971) were text-based and implemented in **FORTRAN**. Early arcades (Sega, Taito) were hardware-limited, using CRT resolutions and joysticks
• **The Microprocessor Revolution**: The **Atari 2600** was a landmark, using the 6502/6507 processor and ROM to allow interchangeable software. Computers like the **Amiga** and **Commodore 64** popularized **BASIC** for home development
• **The PC Era**: The IBM PC democratized development. High-level languages like **C** and the **VGA Mode 13h** (320x200, 256 colors, linear video memory) allowed for more powerful graphics
• **Evolution of Connectivity**:
    ◦ **1980s-90s**: BBS communities preceded the commercial internet
    ◦ **Local Networks**: Use of **IPX** protocols before TCP/IP became standard
    ◦ **Online Milestones**: _Doom_ and _Quake_ popularized online play; **Sega Dreamcast (1998)** was the first console with a built-in 56Kbps modem
• **Mobile & Future**: Smartphones brought back constraints (limited battery, CPU) while enabling always-connected play. Future trends include **AR/VR, Cloud Gaming, and AI-driven development**

## Object-Oriented Programming (OOP) in Games (P3)

OOP is essential for managing the millions of lines of code in modern games by organizing them into **Objects** holding **States** (descriptors) and **Behaviors** (actions)
Core Principles:
• **Cohesion**: Grouping code that contributes to a single task
	• **Functional Cohesion** (doing one thing well) is highly desirable,
	• **Informational Cohesion** (representing set of data and operations) is highly desirable, 
	• **Procedural Cohesion** tasks that need to be done in specific order
	• **Temporal Cohesion** tasks that need to be executed around the same time
	• **Logical Cohesion** 
	• **Coincidental Cohesion** (grouped by chance) is not desirable
• **Coupling**: The "separation of concerns" where objects do not directly modify each other’s internal states
• **Message coupling** is preferred over **Content coupling** (relying on internal details)
• **Encapsulation**: Information hiding via **Getters and Setters** to protect implementation details
• **Abstraction**: Thinking in generalized concepts (e.g., a "Game Console" vs. a specific "PS5")
• **Inheritance**: Creating class hierarchies (Superclasses/Subclasses) to reduce code redundancy (DRY principle)
## Game Programming Patterns
Patterns provide reusable solutions to common development problems
• **Command**: Encapsulates a request as an object, allowing for **Undo/Redo** and dynamic input mapping
• **Flyweight**: Efficiently supports large numbers of objects (e.g., a forest of trees) by sharing common data and keeping only unique parameters (position, tint) per instance
• **Observer**: Defines a one-to-many dependency, so dependents are notified of state changes (e.g., unlocking an achievement when a physics event occurs)
• **Prototype**: Creating new objects by cloning a prototypical instance
• **Singleton**: Ensures a class has only one instance with a global access point
• **State**: Allows an object to change behavior when its internal state changes (Finite State Machines)
• **Game Loop**: The core of a game, processing input, updating game state, and rendering frames
• **Component**: Allows a single entity to span multiple domains (Input, Physics, Graphics) without coupling them
• **Service Locator**: Provides a global access point to a service (e.g., audio) without coupling the user to the concrete implementation
## Game Engines & Architecture

A game engine is a platform for team-based product building, composed of several top-level systems
**Engine Comparison:**

| **Engine**    | **Language**   | **Strengths**                                                   |
| ------------- | -------------- | --------------------------------------------------------------- |
| **Unity**     | C#             | 2D/3D versatility, Ease of use, AR/VR support                   |
| **Unreal**    | C++/Blueprints | AAA-quality rendering (Lumen), Physics (Chaos), Cinematic tools |
| **Godot**     | GDScript/C#    | Open-source, lightweight, node-based architecture               |
| **CryEngine** | C++            | High-end visuals, FPS focus                                     |
| **Frostbite** | C++            | Proprietary (EA), world-class destruction physics               |
| **Cocos**     | C++/JS         | Mobile/Web focus, lightweight 2D                                |
Low-Level Libraries:
• **Graphics APIs**: **DirectX** (Windows/Xbox optimized), **OpenGL** (Cross-platform), **Vulkan** (Low overhead, multithreading)
• **Multimedia**: **SDL** handles cross-platform input, 2D rendering, and audio
Engine Systems:
• **Scene Graph**: A hierarchical data structure specifying relationships between game objects
• **Rendering Engine**: Converts 3D wireframe models into 2D images using a GPU
• **Physics Engine**: CPU-based simulation of physical concepts (fluids, soft bodies, fracturing)
## Artificial Intelligence in Games
Traditionally one of the main research topics in AI (e.g., IBM DeepBlue, AlphaGo)
• **Applications**:
    ◦ **NPCs**: Creating believable movement, reactions, and companions
    ◦ **Design**: Procedural Content Generation (PCG) and Difficulty Estimation
• **Problem Solving Agents**: Defined by an **Initial State**, **Operators** (actions), and a **Successor Function** to explore the **State Space**
• **Key Algorithms**:
    ◦ **Minimax**: Maximizer tries to get the highest score; Minimizer tries for the lowest
    ◦ **Monte-Carlo Tree Search (MCTS)**: Decision-making based on searching combinatorial trees
    ◦ **A***: An informed search that minimizes path cost (Actual Cost + Heuristic)
    ◦ **Neural Networks**: Self-adaptive structures capable of modeling complex real-world scenarios by learning from training data
## Physics & Collision Detection
Based on **Newton’s Laws of Motion** (Inertia, F=ma, Action/Reaction)
Collision Detection Phases
1. **Broad Phase**: Finds _potentially_ colliding pairs using simple volumes. The **Axis-Aligned Bounding Box (AABB)** is common because it is computationally cheap
2. **Narrow Phase**: A refinement step to determine actual intersection and compute contact points
    ◦ **Separating Axis Theorem (SAT)**: Two convex shapes do not intersect if there is an axis where their projections do not overlap
    ◦ **GJK Algorithm**: Computes distance between convex shapes and identifies closest points
Advanced Concepts
• **Convex Hull**: The smallest convex shape containing a concave object
• **Continuous Collision Detection**: Used to prevent **Tunneling** (fast objects passing through obstacles between frames) by computing the **Time of Impact (**tc​**)**
## Automata Theory & FSM
**Finite State Machines (FSM)** are abstract devices following a predetermined sequence of operations
• **Deterministic Finite Automaton (DFA)**: Each input leads to exactly one state
• **Non-Deterministic (NDFA)**: An input can lead to multiple possible state combinations
• **Moore vs. Mealy Machines**:
    ◦ **Moore**: Output depends only on the current state; often results in one clock cycle of delay
    ◦ **Mealy**: Output depends on current state _and_ input; reacts faster (same clock cycle)
## Network Programming
Essential for multiplayer experiences and DLC delivery
• **IP Protocol**: Provides a "cloud" interface for hosts to communicate regardless of physical hardware
• **Addressing**: Classes A, B, and C are **Unicast** (one-to-one). Class D is for **Multicast** (one-to-many)
• **Transport Protocols**:
    ◦ **TCP**: Connection-oriented, reliable (re-transmits lost packets), but slower
    ◦ **UDP**: Stateless, not reliable, but much faster; ideal for real-time games
• **Sockets**: The interface between an application and the network. **SOCK_STREAM** is used for TCP; **SOCK_DGRAM** for UDP
• **Optimization**: Use binary protocols for efficiency and send only the differences between states to save bandwidth

**Analogy for Understanding Patterns**: If building a game is like running a restaurant, the **Game Loop** is the kitchen's clock, the **Objects** are the ingredients, **Cohesion** is keeping the dessert tools away from the meat cleavers, and **Design Patterns** are the standard recipes that ensure every dish is consistent and easy to prepare.

## 