---
layout: post
title: "Two Worlds, One Scene: Architecting Project Echo's Time-State System"
date: 2026-11-09
author: Stan
categories: [Game Development]
published: true
excerpt: >
  Project Echo is built around a simple idea: the player can switch between Past and Present at any moment. But representing two complete states of the same world — with objects, geometry, and puzzle pieces in different positions across both — turns out to be one of the more interesting architectural problems I've had to solve.
---

# Two Worlds, One Scene: Architecting Project Echo's Time-State System

## (What Happens When Your Core Mechanic Is Also Your Hardest Engineering Problem)

The core loop of Project Echo is easy to describe. The player explores an environment and, at any point, can toggle between a Past version of that world and a Present version. Objects may have moved. Doors that were locked might be open. A path that is blocked now might have been clear before. The puzzle emerges from the relationship between what was and what is.

That description makes the mechanic sound simple. And from a player's perspective, it should feel simple. Flip a switch, the world changes.

The engineering underneath that flip is considerably messier.

---

## The Problem Nobody Warns You About

When you have a single game world, state management is already non-trivial. You track where objects are, what has been activated, what the player has collected, what timers are running. Standard stuff.

When your core mechanic requires two complete, independent states of the same world — both of which have to remain valid, consistent, and immediately accessible — you end up with an entirely different class of problem.

The naive approach is to build the game twice. Scene A is the Past. Scene B is the Present. The player switches between them. Done.

Except it isn't done. If the player carries an object from the Present into the Past, what happens to it? If a door in the Past was opened by a switch that doesn't exist yet in the Present, does that door exist in both states? If the player drops something in the Past and switches to the Present, is it still there? What about physics? Sound? Visual transitions?

The seams between the two worlds have to be managed, not ignored. And the more interesting your puzzles become, the more complex those seams get.

---

## Snapshots vs. Layered State

I considered two broad approaches early in Project Echo's development.

The first approach is what I call **snapshot-based state**. Each world has a complete snapshot of its own data. When the player switches, the engine tears down the visible world and rebuilds it from the target snapshot. Fast to reason about, easy to serialize, and each world is fully isolated from the other.

The second approach is **layered state within a single scene**. Every object in the scene has two sets of data — one for Past, one for Present. The world itself doesn't change; only which layer is active changes. The switch is instant because nothing is rebuilt.

Each approach has real trade-offs.

Snapshots are conceptually clean. But rebuilding the scene on every switch is expensive, adds latency to what should feel like an instant transition, and creates awkward moments for things the player is currently holding or standing on. If the player is carrying a crate and switches timelines, what exactly gets rebuilt around them?

Layered state is faster at runtime and handles held objects more naturally — the player and anything they're carrying live outside the state layers, interacting with whichever layer is currently active. The downside is that every system in the game has to understand that objects have two sets of data, not one. That awareness has to be built into physics, animation, rendering, audio, and anything else that cares about where an object is or what state it's in.

For Project Echo, I went with **layered state**, primarily because the player experience demanded it. The transition between Past and Present has to feel immediate. Any perceptible pause — even a frame or two — breaks the feel of the mechanic. Rebuilding a scene, however efficiently, introduces that pause.

---

## What Actually Gets Layered

Not everything in Project Echo needs dual state. That distinction matters a lot for performance and complexity.

**Objects that change between timelines** — movable crates, switches, doors, destructible elements, NPCs or traces of them — have both a Past and Present representation. They are stateful in both directions.

**Objects that are identical across timelines** — the basic geometry of floors, walls, and ceilings in areas that haven't changed — have a single representation. No duplication needed.

**The player themselves** lives outside both layers. They move through whichever layer is active. What they carry is an interesting edge case I'll come back to.

**Persistent objects** — items the player has interacted with in a way that changes both timelines — exist in a third category. These are probably the most complex element of the whole system, because they represent the player's impact on history.

Categorizing objects correctly up front has been one of the most valuable parts of the design process. Every object I build now gets an explicit tag: timeline-specific, timeline-shared, or player-persistent. That tag determines how much of the state system it needs to know about.

---

## The Object That Crosses Time

The most interesting edge case in Project Echo is a simple question: what happens when the player moves an object from one timeline and drops it in the other?

This is not a bug. It's a feature. It's the core puzzle mechanic.

A crate that only exists in the Past can be carried into the Present if the player picks it up before switching. When dropped in the Present, it becomes a Present-layer object. It now blocks paths or activates switches in a timeline it didn't originally exist in.

Managing that gracefully required building a small state-transfer system. When an object moves from the player's inventory into an active timeline layer, it registers itself with that layer, sets its Past or Present data accordingly, and becomes subject to that layer's state rules going forward. It is effectively immigrating from one timeline to the other.

This gets surprisingly expressive as a puzzle element. Puzzles can be built around objects that need to be relocated across timelines. The order in which the player switches timelines and drops objects becomes part of the solution.

---

## What This Means for Project Echo

The architecture has shaped the design in ways I didn't fully anticipate when I started.

Because layered state makes the transition instant, I've been able to design puzzles where the player switches timelines several times in quick succession to understand what has changed. That rapid comparison is itself a kind of puzzle solving. Players are essentially diffing two versions of the same world to find the meaningful differences.

The timeline-crossing object mechanic opened up an entire category of puzzles I hadn't originally planned for. The architecture made it possible, and the design followed.

There are still open problems. Performance profiling will eventually tell me whether keeping every stateful object dual-loaded is sustainable at the level sizes I want. The transition visual effect — still in early design — needs to do work that the architecture can support. And the serialization story for save states is non-trivial when every object has two positions.

But the core system works. I can switch between timelines instantly, carry objects across them, and build puzzles that depend on the relationship between past and present states. That's the foundation everything else is built on.

---

## Final Thought

The hardest part of building a mechanic like Project Echo's isn't the idea. The idea is straightforward. The hard part is deciding, early, what kind of system will carry that idea all the way through production without accumulating so much complexity that it collapses.

**Get the architecture right before you build the first puzzle, because the architecture determines which puzzles are even possible.**