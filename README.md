# LLM-Augmented Navigation Agent: `SentinelMeetingAgent` & `Reasoner`

This coding sample implements an LLM-guided navigation system in a simulated urban environment.  
The agent can interpret aerial images, build a semantic scene graph, avoid patrolling sentinels, and navigate safely to the meeting location.

---

## Overview

### Reasoner (ThinkingModule)

Reasoner  
├─ Maintains global Hybrid Scene Graph (HSG)  
├─ Uses LLM prompts for:  
│ ├─ Action planning  
│ ├─ Scene-graph reconstruction from language  
│ └─ Safe-path proposal & route refinement (vision + text)  
└─ Validates routes against sentinel locations  

### SentinelMeetingAgent (BaseNavigationMeetingAgent)

SentinelMeetingAgent  
├─ Processes simulator observations and app events  
├─ Tracks sentinel poses and emergency states  
├─ Requests aerial grid-map images & refined routes  
└─ Executes navigation or emergency avoidance via occupancy maps  


---

## Reasoner: LLM-Based Spatial Reasoning

- **Action Planning**  
  Generates high-level decisions using prompts filled with current time, position, and intent.

- **Scene-Graph Construction (HSG)**  
  - From conversation: reconstructs semantic nodes, relations, and properties.  
  - From aerial images: extracts subgraphs and merges them into a global scene graph.

- **Safe-Path Proposal**  
  Serializes the current HSG and queries the LLM for a safe symbolic path, identifying missing information when needed.

- **Route Refinement (Image + Text)**  
  Combines last route, sentinel positions, and grid-map images to request refined waypoints from the LLM.

- **Robust JSON Parsing**  
  Handles malformed LLM outputs through automatic re-prompting and recovery.

---

## SentinelMeetingAgent: Embodied Navigation & Emergency Handling

- **Observation Processing**  
  Updates internal states based on visible sentinels, app messages, and action outcomes.

- **Emergency Detection & Avoidance**  
  When a sentinel is near, enters emergency mode and samples safe positions using occupancy-grid validation and probabilistic scoring.

- **Route Refinement Triggering**  
  When danger overlaps with the current route or progress stalls, the agent:  
  1. Requests an aerial grid-map image  
  2. Passes it to the Reasoner for waypoint refinement  
  3. Requests the app to compile an executable refined route

- **Normal Navigation**  
  Performs LLM-guided discussion planning or city-level navigation until reaching the meeting place.

---
