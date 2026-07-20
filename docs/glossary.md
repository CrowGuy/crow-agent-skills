# SDLC Core Glossary

This document defines the **"Leading Words"** of our development process to align engineering taste and ensure a shared understanding between human developers and AI agents.

## 1. Planning & Wayfinding
- **Technical Fog**: Refers to large, fuzzy, and unformed concepts. When a task is **"wrapped in fog,"** the path to the destination is not yet visible, requiring decomposition via `/wayfinder`.
- **Smart Zone**: The cognitive interval where an AI Agent maintains high-quality reasoning and logic. Tasks that are too large or have excessive context can **"blow out of the smart zone,"** leading to a decline in output quality.
- **Shared Map**: A visualized plan created on the repository's issue tracker (e.g., GitHub Issues), recording all sub-tasks and their **blocking relationships**.
- **Blocking Relationships**: Explicitly marking on the map which decisions or research must be completed before subsequent tasks can proceed.
- **Prototype**: A **"cheap, rough, and concrete artifact"** (such as UI code or logic snippets) created to raise the fidelity of discussion, allowing for quick reaction and course correction.

## 2. Specification & Tickets
- **Specification (Spec)**: A broader term than a PRD. It defines the destination by encompassing both product requirements and technical implementation details.
- **Defining Constraint**: The single fact or core limitation that makes a skill or task behave differently from the default. (e.g., `/to-spec` synthesizes existing knowledge without re-interviewing the user).
- **Facts vs. Decisions**: **Facts** are existing information the AI discovers by exploring the codebase; **Decisions** are choices that must be explicitly made by the user.
- **Exploration Issue**: A single-session unit of *exploration* created by `/wayfinder` **before** a Spec exists, to reduce Technical Fog. Its type is one of `research`, `prototype`, `grilling`, or `human-action`. Distinct from a Ticket — an Exploration Issue produces facts and decisions; it is not implementation work.
- **Implementation Ticket** (a.k.a. **Ticket**): The decomposition of a **Spec** into the smallest units of *implementation* work that can be completed within a **single agent session**. Produced only **after** `/to-spec`. The naming boundary lets an agent judge an item's execution boundary from the noun alone: Exploration Issue = before the Spec, Implementation Ticket = after it.

## 3. Implementation
- **Test-Driven Development (TDD)**: Following the **"Red-Green"** cycle (failing test → passing test). In this workflow, refactoring is moved to the Code Review stage to avoid overloading the implementation session.
- **Seam**: Pre-agreed connection points in the code where tests can be inserted or logic can be safely replaced.
- **Tracer Bullet**: A minimal implementation that pierces through all system layers (from UI to database) to validate the feasibility of the architecture.
- **Single Test Files**: Running small, specific tests frequently during development, rather than executing the **full test suite** every time.

## 4. Quality & Refactoring
- **Two-Axes Review**: A code review process evaluated on two distinct planes:
    - **Standards Axis**: Checks if the code conforms to the repository's documented **coding standards**.
    - **Spec Axis**: Checks if the code faithfully implements the originating requirement or Spec.
- **Code Smells**: Classic engineering concepts (from Martin Fowler) that are deep in the agent's prior knowledge:
    - **Primitive Obsession**: Overuse of primitive types (strings, integers) instead of dedicated objects.
    - **Data Clumps**: Groups of data items that always appear together and should be encapsulated.
    - **Message Chains**: Long navigation calls (e.g., `a.b().c().d()`).
    - **Feature Envy**: A method that is overly dependent on data from another class.
