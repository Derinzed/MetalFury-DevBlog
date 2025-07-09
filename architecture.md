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
In MetalFury, runtime behavior is coordinated through a dual-system architecture built around **Events** and **Actions**. These systems enable a clean separation between **immediate responses** and **deferred behavior**, keeping gameplay logic transparent, testable, and decoupled.

---

### Events: Instantaneous, Broadcast-Style Communication

The **Event System** is built for **immediate, synchronous communication** between systems. When something happens *right now*—a collision occurs, a unit takes damage, or an input is received—it’s emitted as an event.

Events in MetalFury follow a classic **publish-subscribe** pattern:

- Systems **subscribe** to specific event types using type-safe member function handlers  
- When an event is **published**, all subscribed handlers are immediately invoked in order  
- Handlers are bound with exact type signatures, ensuring predictable behavior  
- All execution happens **during the frame** in which the event is fired—no queuing, no delay  

This model makes event-driven logic ideal for handling **reactions** to gameplay moments in a modular way—without forcing systems to know about each other

> **If it needs to happen now, it’s an Event**


### Actions: Deferred, Intent-Based Execution

When logic needs to happen **later**, or when systems must request work *they themselves don’t execute*, MetalFury uses **Action Delegation**

An **Action** is a self-contained unit of behavior. It encapsulates:

- A function or method to execute  
- Any parameters required  
- Optionally, a return result  

Actions are submitted to a centralized **Action Queue**, where they can be prioritized, ordered, or delayed

Unlike events, **actions do not broadcast**. They’re **targeted and explicit**—executed in well-defined queues by the systems that own them

> **Important design rule**  
> The **requester must already have access** to the system it’s delegating an action to  
> This avoids global discovery and reinforces modular access control

Use **Actions** when:

- You want to **defer** a task to the next update cycle  
- You want to **schedule** behavior across systems  
- You want **return values** from operations  
- You’re **batching** work or executing logic outside the main frame loop  


# From Editor To Game
As stated in the MetalFury Manifesto, the editor is not an afterthought—it’s a first-class citizen. In fact, it is the first game the engine produces.

MetalFury supports multiple editors: some may be general-purpose, others highly specialized. Regardless of their scope, all editors are built using the same MetalFury API available to any game project. This consistency raises an important question:
“How do you transition from editing a game to running it?”

The MetalFury Editor is designed to answer that by fully managing a game's workspace lifecycle. It initializes boilerplate source files, sets up the project structure, and supports hot loading, allowing you to modify and debug the game at runtime.

Editor vs. Game Mode
When a new game project is created, the editor:
Initializes a dedicated workspace.
  Generates the necessary project files and boilerplate source code.
  Prepares the project to support two build modes:
  Release Mode: Compiles the game into a standalone executable, ready for distribution.
  Editor Mode: Compiles the game logic as a hot-loadable dynamic library (DLL), which is then ingested and executed inside the editor environment.
This architecture empowers rapid iteration, live editing, and a clean separation between development-time tools and runtime behavior—all while staying true to MetalFury’s modular, transparent, and testable philosophy.


##  Lua as the Game Scripting Language

MetalFury uses **Lua** as its primary scripting language for game logic. This decision aligns with MetalFury's design principles—especially those favoring **runtime flexibility**, **clear separation of systems**, and **opt-in dynamism**.

Lua is lightweight, embeddable, and fast enough for real-time scripting, while remaining simple and expressive for designers and gameplay programmers. Game systems expose scriptable APIs that Lua scripts can use to:

- Control game flow and state  
- Define behaviors, interactions, and dialogue  
- Hook into systems like input, animation, and events  
- Extend or override gameplay without recompiling core logic  

By default, the engine provides bindings to key systems through a stable, sandboxed interface. Projects are free to define additional bindings, embed their own interpreters, or layer scripting environments as needed.

>  Lua empowers iteration, customization, and dynamic gameplay—without compromising the engine’s structural clarity.  
> While not always practicle, LUA provides a clean and rapid way of developing games that do not need complex and custom system creation.

---

##  Plugin Loading and Dynamic Extension

MetalFury supports **runtime plugin loading** as a first-class mechanism for engine extension and modular development. Plugins allow developers to add or override functionality **without modifying the engine core**.

### Plugin Lifecycle

- Plugins are loaded **after all core systems have been initialized**
- The engine **tracks registered plugins**, allowing systems to query what functionality is present
- Plugins are dynamically discovered and integrated via a common interface  

This architecture enables features like:
- Modular gameplay features  
- Editor extensions  
- Custom system overrides  
- Platform or service integrations  

### Development vs Final Project Use

- During **engine development**, each plugin must provide a **DLL**, **LIB**, and **header file** so it can be linked and extended natively
- In **final game projects**, only the **DLL** is required at runtime

This ensures a smooth workflow during engine extension, while keeping deployment minimal and clean for shipped titles.

> Plugins are optional—but when used, they’re treated as first-class citizens in the MetalFury ecosystem.

>  **NOTE**  
>   Plugins are designed for runtime extension, which makes the incompatable with the compile time setup of core systems.  If new core systems are wished to be distributed this can be facilitated by distributing the new system's source and header files into the project and registering them using the standard MetalFury API during engine setup.

## The Two-Part Contract: Application and Engine

MetalFury is built on a deliberate separation between **the Engine** and **the Application**. This two-part agreement defines a clear boundary:

- **The Engine** is a self-contained, reusable framework and library  
- **The Application** is any consumer of that engine—whether a game, editor, or custom tool  

This separation of concerns is fundamental to MetalFury’s modular and developer-first design philosophy.

---

### Opt-In by Design

Because the engine is *consumed* by the application layer, the application developer retains full control over what is used, extended, or overridden. This offers several powerful advantages:

- Developers **own their entire project** structure  
- **Only the systems they need** are included—core systems are *opt-in*  
- Everything, including the **main game loop**, is customizable  
- Core systems themselves are **extensible** and replaceable  
- The engine never assumes your architecture—it adapts to it

This flexibility empowers teams to build highly specialized or unconventional applications without battling against built-in assumptions.

---

### Application as Coordinator

The second key benefit is architectural clarity: the **application acts as the orchestrator**.

- It connects and manages engine systems  
- It routes events and schedules actions  
- It defines all gameplay logic and behavior  

Meanwhile, **engine systems remain completely agnostic** to the application. They expose clean interfaces, but do not know or care about game-specific logic.

This strict separation:
- Keeps the engine **clean, reusable, and testable**  
- Makes gameplay logic **explicit and domain-specific**  
- Enables the same engine to power vastly different projects  

---

### Bridging the Gap with the Editor

This model does come with an **inherently higher barrier to entry**—especially for new developers. But MetalFury addresses this with its built-in **Editor**, included as part of the core engine.

The Editor:
- Sets up projects and boilerplate automatically  
- Manages dependencies and system registration  
- Abstracts away advanced coordination until it’s needed  

When you're ready to go deeper, the engine architecture is already there—waiting to be shaped to your needs.

> You control the contract. The engine serves your vision, not the other way around.
