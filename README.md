# LLM-Guided Spatial Reasoning & Navigation Agent  
### Coding Sample Overview

This code implements an intelligent navigation agent in a simulated urban environment.  
The agent must reach a target location while avoiding patrolling guards (“sentinels”), interpreting images, and refining routes in real time. The system consists of two tightly-connected modules:

---

## 1. Reasoner (LLM-based High-Level Module)

**Goal:** Provide *semantic understanding* and *global reasoning*.

### Key Capabilities
- **Scene Graph Construction**  
  - Reconstructs a Hybrid Scene Graph (HSG) from:
    - Aerial images  
    - Conversation excerpts  
  - Uses an LLM to output structured JSON describing objects, relations, and connectivity.

- **Safe Path Planning**  
  - Serializes the current scene graph and asks the LLM to propose a safe symbolic route.
  - If information is missing, the LLM identifies what should be queried next.

- **Route Refinement (with image)**  
  - Receives a grid-map image and last route.
  - Uses an LLM (vision + text) to refine waypoints around sentinel positions.

- **Robust JSON Parsing**  
  - Automatically re-prompts the LLM if output formatting is incorrect.

This module focuses on **global reasoning, multimodal prompting, and structured planning**.

---

## 2. SentinelMeetingAgent (Embodied Navigation Module)

**Goal:** Execute movement safely and efficiently inside the simulator.

### Responsibilities
- **Observation Processing**
  - Tracks sentinel visibility and updates risk level.
  - Receives aerial images or refined routes from the in-simulation “app”.

- **Emergency Handling**
  - Detects sentinel proximity and switches to an avoidance mode.
  - Performs short-range motion planning using an occupancy grid:
    - Validates positions  
    - Scores candidate locations  
    - Selects safe points probabilistically  

- **Route Refinement Triggering**
  - Requests new aerial images or refined routes when:
    - Current path intersects danger  
    - The agent gets stuck  
    - Post-emergency recovery indicates deviation  

- **Task Execution**
  - Discussion mode (LLM-generated plan)  
  - Navigation mode (city-level route following)  
  - Task completion when arrival is detected  

This module focuses on **perception, safety control, and real-time decision making**.

---

## 3. What This Coding Sample Demonstrates

- Integration of **LLM reasoning** with a **physical agent**  
- Real-time **scene-graph construction** from vision + language  
- Automatic **safe-path planning** combining LLM and classical spatial algorithms  
- Practical **error-handling**, robust JSON parsing, and fallback strategies  
- A clean separation between:
  - *Reasoning* (symbolic, LLM-driven)  
  - *Acting* (navigation, motion planning)  

---

## 4. Summary

The code presents a compact but complete pipeline for LLM-augmented embodied navigation:
- The **Reasoner** thinks globally  
- The **Agent** acts locally and safely  

This showcases skills in multimodal LLM use, agent design, graph reasoning, and robotics-style navigation—suitable for demonstrating strong coding and system-engineering ability in a graduate application.

