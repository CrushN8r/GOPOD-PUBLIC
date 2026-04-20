# GOPOD-PUBLIC

  ## Overview

  GOPOD is a deterministic multi-plane control system for robot orchestration.

  This public repository documents the non-sensitive architecture, control model, and integration boundaries. It does not include the private runtime core, deployment state, or machine-local execution logic.

  GOPOD is not a chatbot, robotics demo, or autonomous agent sandbox. It is a constrained control system with explicit routing and execution boundaries.

  ## System Model

  GOPOD is organized as a 3-plane architecture:

  1. **Jetson / Goverlord**
     - orchestration and decision layer
     - produces structured ACTION objects
     - does not talk to hardware directly

  2. **T560 / Gomad**
     - deterministic actuator layer
     - exposes the `/api-sdk` HTTP surface
     - executes robot actions only

  3. **Codex**
     - repository mutation layer
     - used for code operations only
     - not part of runtime decision logic

  ## High-Level Architecture

  ```text
  +---------------------+         +---------------------+         +---------------------+
  | Jetson              |         | T560                |         | Codex               |
  | Goverlord           |         | Gomad               |         | Repo Mutation Layer |
  | Decision / Routing  |         | Actuation / /api-sdk|         | Code Execution Only |
  +---------------------+         +---------------------+         +---------------------+

  Jetson emits ACTION objects only.
  T560 executes robot actions only.
  Codex executes repository actions only.

  ## Execution Flow

  Jetson -> goverlord_exec -> validator -> router -> (T560 | Codex)

  All execution is expressed as a structured ACTION object:

  {
    "type": "robot" | "code",
    "target": "string",
    "action": "string",
    "params": {}
  }

  ## Architectural Constraints

  - No direct robot control from Jetson tools or orchestration logic
  - No direct execution from LLM-facing tools
  - No side-effect execution outside the core execution path
  - All actions must be validated before routing
  - Router logic is deterministic and mapping-only
  - T560 contains no business logic or planning
  - Codex is isolated to repository mutation tasks

  ## Public Scope

  This repository is intended to expose:

  - architecture
  - control contracts
  - non-sensitive interfaces
  - high-level system layout

  The following remain private:

  - deployment-specific runtime code
  - machine-local configuration
  - active control infrastructure
  - operational state and internal tooling

  ## Status

  Active development.

  Private execution core remains separate.
  
