# About
Though MetalFury is a philosophy first, it also has a practical implementation.  
This implementation is built fully in accordance with the MetalFury Manifesto, and expresses its principles through deliberate, modular architecture.  
Here you can explore the structure of the engine and how its systems are composed.

# Systems
At the heart of MetalFury is the concept of a **system**.  
A system is a **self-contained module**, designed to perform a specific task in a predictable, testable, and replaceable way. Systems may perform complex logic, but each must have a **clear, bounded responsibility**.

There are two primary categories of systems in MetalFury:

## Core Systems
**Core systems** perform responsibilities that exist **independent of any particular entity**.  
They manage global or game-wide concerns—things like:

- Rendering
- Audio output
- Physics simulation
- Input processing
- File I/O
- Scripting interfaces

A core system may **serve entities**, but it does not exist *because of* entities.  
Core systems are typically registered at engine startup and injected into other systems (or scripts) as needed.

## ECS Systems
**ECS systems** are systems that operate **exclusively on entities**.  
Their entire purpose is to read, update, or react to **component data** within the ECS framework. Examples include:

- MovementSystem
- DamageSystem
- HealthSystem
- AnimationSystem
- TargetingSystem

Each ECS system declares which component archetypes it cares about and processes only those. ECS systems never reach outside the ECS world to manage global state—they are **pure consumers and transformers of entity state**.

---

## Core System ↔ ECS System Interaction

Core systems and ECS systems are intentionally decoupled.  
**They do not assume each other’s existence.**  
However, when necessary, **the application may explicitly connect the two** using **dependency injection**.

For example:
- A `RenderSystem` (ECS) may be passed a `Renderer` (core) instance to issue draw calls.
- An `AISystem` may receive a `PathfindingService` (core) to calculate movement routes.

This connection must always happen **at the application level**, not inside the engine or the ECS system itself.  
This ensures that:

- Systems remain modular and testable
- Dependencies are explicit and opt-in
- The engine stays general-purpose and non-prescriptive

**If a system requires another system, it must be passed to it, not discovered globally.**

Alternatively, events may be used to declare changes, or requests in global states, thus allowing inter system communication without any application level binding.  

```cpp
// Example: ECS system receiving a core system via DI
auto renderer = std::make_shared<RendererSystem>();
auto renderECS = std::make_unique<RenderSystem>(renderer);
ecs->registerSystem(std::move(renderECS));
```

# Event Driven, Action-Delegated Design
