# 🛠️ The MetalFury Manifesto: A Design Philosophy for Game Engines

## Purpose
**MetalFury is not a codebase—it’s a philosophy for building powerful, sustainable, and adaptable game engines.**  
It is built to empower the creation of complex, simulation-rich, and system-driven games with clarity, speed, and control.

This manifesto defines the core principles of MetalFury, independent of any specific language, framework, or implementation.

---

## ⚙️ 1. Modular by Nature
Every system should be a self-contained unit, independently testable, replaceable, and composable.  
- No module should assume the presence or shape of another.  
- Clear, narrow interfaces define cooperation between systems.  
- The engine is a *collection of small, sharp tools*, not a single monolith.

> “Tightly coupled code rots. Loosely coupled systems evolve.”

---

## ⚡ 2. Performance as a First-Class Citizen
Performance is not an optimization—it’s a design constraint.  
- Prioritize cache efficiency, data-oriented design, and lightweight communication.  
- Avoid hidden costs. Favor transparency over ease when ease hides complexity.

> “Predictable is better than magical.”

---

## 🔄 3. Compile-Time by Default, Runtime When It Matters
- Prefer static configuration, structural typing, and early validation.  
- Use dynamic behavior where runtime flexibility is essential (e.g. scripting, plugins).  
- Everything should be opt-in, not imposed.

> “Design for control, not convenience.”

---

## 🔁 4. Extendable and Replaceable at Every Layer
From input to rendering to AI to simulation, every core system must be swappable.  
- The engine owns no assumptions about your game’s architecture.  
- Custom logic and systems are not "hacks"—they are *first-class citizens*.

> “The engine should shape to the game, not the other way around.”

---

## 🧪 5. Built for Testing, Always
If it can’t be tested, it can’t be trusted.  
- Every module should be independently verifiable.  
- No single path through the engine should be required to validate behavior.  
- Automated builds and tests are non-negotiable.

> “A game engine is not just for running games—it’s for testing them.”

---

## 🧱 6. Entity-Component-System as Foundation, Not Dogma
Entity-Component-System (ECS) is favored for its flexibility and scalability, but:
- Use ECS because it fits the problem—not because it’s trendy.
- Design ECS around clarity and decoupling, not around theoretical purity.
- Prefer systems that **express game logic cleanly**, even if it means bending the model.

> “The architecture serves the game. Not the other way around.”

---

## 🔧 7. Tools are Core, Not Optional
An engine is not complete without tooling.  
- Editors, debuggers, visualizers, profilers—these are part of the engine, not accessories.  
- Tooling must consume the same APIs the game does. No hidden backdoors.

> “If a system can’t be inspected, it can’t be trusted. If it can’t be edited, it can’t be improved.”

---

## 📦 8. One Engine, Many Use Cases
MetalFury is not a one-game engine. It must adapt across genres, styles, and timelines:  
- Abstract assumptions (e.g. rendering, input, data) into interfaces.  
- Make scripting and configuration language-agnostic where possible.  
- License, extend, or embed without rewriting the core.

> “What you build today should empower what you build tomorrow.”

---

## 🧠 9. Thoughtful Over Clever
Avoid premature abstraction, over-optimization, or magical behavior.  
- Favor simple systems with deep leverage.  
- Readability, predictability, and maintainability win over cleverness.

> “The best engine is one you don’t have to think about while using.”

---

## ⏳ 10. Designed to Outlive Its Creators
An engine’s value isn’t in what it runs today—but in how gracefully it evolves.  
- Design for human change: new contributors, new platforms, new games.  
- Build documentation into the process.  
- Leave room for growth, iteration, and reinvention.

> “If you stop working on it, it should still be usable. If someone else picks it up, they should understand it.”

---

## MetalFury Is a Promise
To yourself. To your games. To your future collaborators.  
A promise that the tools you build will empower rather than constrain.  
A promise that good design, clear thinking, and strong architecture will last longer than trends.

**MetalFury is not the code. MetalFury is the clarity behind the code.**
