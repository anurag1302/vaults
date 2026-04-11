### Introduction

Agentic AI is a type of AI that works to complete a goal/task/work, without minimal human intervention.
It plans, researches, adapts, asks questions and eventually completes the goal.

### Key Characteristics

- Autonomous
- Goal Oriented
- Planning
- Reasoning
- Adaptability
- Context Awareness

```mermaid
flowchart TD
    A[Agentic AI Core] --> B[Autonomous Execution]
    A --> C[Goal-Driven]
    A --> D[Tool Use]
    A --> E[Planning & Decomposition]
    A --> F[State & Memory]
    A --> G[Reflection & Self-Correction]
    A --> H[Looping / Iteration]
    A --> I[Environmental Awareness]

    B --> B1["Acts without step-by-step prompts"]
    C --> C1["High-level objective → own sequence"]
    D --> D1["Calls external functions\n(APIs, code, search, DB)"]
    E --> E1["Breaks goals into sub-tasks"]
    F --> F1["Short-term (context) +\nLong-term (vector DB)"]
    G --> G1["Detects mistakes, retries"]
    H --> H1["Reason → Act → Observe cycle"]
    I --> I1["Reads file system, errors,\nAPI responses"]
```