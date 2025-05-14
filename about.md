# The MetalFury Manifesto: A Design Philosophy for Game Engines

## Purpose
**MetalFury is not just a codebase—it’s a philosophy for building powerful, sustainable, and adaptable game engines.**  
It is built to empower the creation of complex, simulation-rich, and system-driven games with clarity, speed, and control.
Game and game engine code should stop being viewed as "made to work" and instead should be made with elegance and intent, with a vision for the future.

This manifesto defines the core principles of MetalFury, independent of any specific language, framework, or implementation.

---

## 1. Modular by Nature
Every system should be a self-contained unit, independently testable, replaceable, and composable.  
- No module should assume the presence or shape of another.  
- Clear, narrow interfaces define cooperation between systems.  
- The engine is a *collection of small, sharp tools*, not a single monolith.
- If a module cannot exist on its own, then it has no place in the whole.

> “Tightly coupled code rots. Loosely coupled systems evolve.”

---

## 2. Performance as a First-Class Citizen
Performance is not an optimization—it’s a design constraint.  
- Prioritize cache efficiency, data-oriented design, and lightweight communication.  
- Avoid hidden costs. Favor transparency over ease when ease hides complexity.

> “Predictable is better than magical.”

---

## 3. Compile-Time by Default, Runtime When It Matters
- Prefer static configuration, structural typing, and early validation.  
- Use dynamic behavior where runtime flexibility is essential (e.g. scripting, plugins).  
- Everything should be opt-in, not imposed.

> “Design for control, not convenience; but there is convenience in control.”

---

## 4. Extendable and Replaceable at Every Layer
The engine’s primary concern is not to implement systems, but to manage, extend, and validate them.
- The engine owns no assumptions about your game’s architecture.
- The engine owns no assumptions about your game's system requirements.  
- Custom logic and systems are not "hacks"—they are *first-class citizens*.
- The role of the engine is to support the creation and validation of systems, while ensuring conformance.

> “The engine should shape to the game, not the other way around.”

---

## 5. Built for Testing, Always
If it can’t be tested, it can’t be trusted.  
- Every module should be independently verifiable.  
- No single path through the engine should be required to validate behavior.  
- Automated builds and tests are non-negotiable.

> “A game engine is not just for running games—it’s for testing them.”

---

## 6. Architecture is a Means, Not an End
Architectural patterns are tools, not rules. A good engine favors solutions that serve the games they wish to make—not the model.
- Use ECS because it fits the problem—not because it’s trendy.
- Engine design is around clean, efficient, performant code, not around theoretical purity.
- Prefer systems that **express game logic cleanly**, even if it means bending the model.

> “The architecture serves the game. Not the other way around.”

---

## 7. Tools are Core, Not Optional
An engine is not complete without tooling.  
- Editors, debuggers, visualizers, profilers—these are part of the engine, not accessories.  
- Tooling must consume the same APIs the game does. No hidden backdoors.
- The editor is the first game an engine should produce.
- If a tool reveals a limitation, the engine must evolve to meet it

> “If a system can’t be inspected, it can’t be trusted. If it can’t be edited, it can’t be improved.”

---

## 8. One Engine, Many Use Cases
MetalFury is not a one-game engine. It must adapt across genres, styles, and timelines:  
- Abstract assumptions (e.g. rendering, input, data) into interfaces.  
- Make scripting and configuration language-agnostic where possible.  
- License, extend, or embed without rewriting the core.

> “What you build today should empower what you build tomorrow.”

---

## 9. Thoughtful Over Clever
Avoid premature abstraction, over-optimization, or magical behavior.  
- Favor simple systems with deep leverage.  
- Readability, predictability, and maintainability win over cleverness.

> “The best engine is one you don’t have to think about while using.”

---

## 10. Complex simplicity
The best engines are not those with the fewest parts, but those whose parts are clear, composable, and powerful.
They are easy to begin with, and deep enough to never outgrow.  
- Prioritize human understanding at every level.
- Build APIs that are simple, not simplistic.
- Make everything customizable, but nothing mandatory.

> “An engine that grows with the developer, outlives any game.”

---

## MetalFury Is a Promise
To yourself. To your games. To your future collaborators.  
A promise that the tools you build will empower rather than constrain.  
A promise that good design, clear thinking, and strong architecture will last longer than trends.

**MetalFury is not the code. MetalFury is the clarity behind the code.**
