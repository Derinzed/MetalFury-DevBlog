# About
Though MetalFury is a philosophy, it also has an implimentation.  This implimentation is built on the MetalFury manifesto's principles.  Here you can read about the core architecture.

# Systems
The heart of MetalFury is the definition of a system.  A system is a sigular module designed to perform a specialized task.  These can be large tasks, but they should be well defined. Within this, there are two kinds of systems

## Core Systems
A core system is a system that performs a task who's responsibility exists beyond the scope of any singular entity.  For instance, rendering, audio, physics.  All of these things may be used by entities, but their responsibility exists beyond just entities.

## ECS Systems
ECS systems are systems whos responsibilitys lie soley with the entities they service.  Examples of this may be health, damage, attack, movement.  Their sole purpose is to perform actions on entities themselves

## Core Systems and ECS Systems Interactions
