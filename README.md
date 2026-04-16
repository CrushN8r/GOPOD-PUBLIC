# GOPOD-PUBLIC

## Overview

GOPOD is a modular robotics + AI orchestration system designed to control multiple robots from a single Jetson-based brain.

This public repo showcases:

* System architecture
* High-level design
* Selected non-sensitive components

## Core Concept

```
Robots → wire-pod (runtime) → GOPOD (orchestration) → Goverlord (AI brain)
```

## Features

* Multi-robot coordination
* Local AI orchestration (multi-LLM ready)
* Web-based control interface
* Deterministic network architecture

## Status

Active development (private core system).

## Notes

* Full system remains private
* This repo is for demonstration, collaboration, and visibility

## Contact

For collaboration or access inquiries, reach out via GitHub.

## UPDATE!!

# GOPOD — Gomad + Goverlord

**Distributed Control System for Multi-Robot Orchestration**  
T560-Gomad = CLOSED Actuator Fabric  
Jetson-Goverlord = ACTIVE Control Intelligence  

Hard architectural boundary enforced. No leakage. No exceptions.

---

### Hard Architectural Contract (Locked)

- **T560-Gomad** = CLOSED (stateless actuator fabric)  
  - All low-level execution (wire-pod, Vector SDK, Moorebot ROS, etc.)  
  - HTTP-only surface: `http://192.168.1.6:8080/`  
  - Zero business logic, zero decision making

- **Jetson-Goverlord** = ACTIVE (pure decision + orchestration layer)  
  - All intelligence, planning, and routing lives here  
  - **100% of robot execution must flow through T560**  
  - No direct hardware, no direct ROS, no direct SDK calls from Jetson

This contract is now **non-negotiable**.

---

### Quick Start (Deterministic Brain — Option A)

```bash
# 1. Clone & setup
git clone https://github.com/crushn8r/GOPOD-PUBLIC.git
cd GOPOD-PUBLIC

# 2. Install Gomad/Goverlord toolkit (run on both machines)
cd tools/gomad-goverlord-toolkit
./install-toolkit.sh   # (or follow the one-liner in tools/)

# 3. Run the deterministic pipeline (Jetson)
cd jetson-goverlord
python3 goverlord_core.py vector1 say text "Hello from the brain"
