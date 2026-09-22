---

layout: post
title: "Number Push — Building a Procedural Puzzle Level Generator"
date: 2026-09-11
author: Stan
categories: [Game Development]
published: true
featured: true
excerpt: >
    Number Push is a free puzzle game built to showcase a procedural level-generation system that can create, solve, measure, and reproduce playable Sokoban-style puzzles. Explore how automated solving, difficulty profiles, and deterministic seeds can turn random generation into a system for creating continuously updated game content.
---

# Number Push — Building a Procedural Puzzle Level Generator

Number Push is a free puzzle game I'm developing, but the game itself is only part of the project. The bigger goal is to demonstrate a **procedural level-generation system** capable of creating, testing, and reproducing playable puzzle levels.

For developers, that is the part of Number Push that interests me most. Instead of manually designing hundreds or thousands of puzzles, the generator can construct candidate levels, determine whether they are solvable, measure their difficulty, and produce levels that fit a specific difficulty profile. The game then becomes the front end for that technology.

## What Is Number Push?

Number Push is based on the classic Sokoban concept of pushing crates around a board. The difference is that each crate has a number associated with it. That number determines **how many spaces the crate moves when it is pushed**. This creates a very different puzzle dynamic from traditional Sokoban.

A push doesn't necessarily move a crate one square. A crate numbered 3 moves three spaces when pushed, for example. That means the player has to think not only about where a crate needs to go, but also about whether the crate can be positioned correctly before making the next push. A single push can move a crate a significant distance, block another route, or completely change the player's available options.

That makes Number Push particularly interesting from a procedural-generation perspective. The generator can't simply create a board that looks reasonable. It has to create a board that works with the unique movement rules of the game.

## The Basic Generation Pipeline

The Number Push generator follows a basic pipeline:

1. Select a difficulty profile.
2. Generate a candidate board.
3. Place walls, crates, goals, and crate movement values.
4. Validate the board.
5. Solve the puzzle.
6. Measure the resulting solution.
7. Compare the result against the requested difficulty.
8. Keep or reject the level.
9. Repeat until a suitable level is found.

The important idea here is that **generation and validation are separate processes**. The generator is allowed to create candidates that aren't good enough. Those candidates are simply rejected. That is much more practical than trying to write generation rules that guarantee every possible board will be perfect.

## Difficulty Profiles

One of the first things the generator needs to understand is what "easy" or "hard" actually means. Simply making a larger board doesn't necessarily make a puzzle harder. Likewise, adding more crates doesn't automatically create a better challenge.

Number Push uses difficulty profiles that establish target characteristics for generated levels.

A profile can define things such as:

* Board size
* Number of crates
* Number of walls
* Crate movement values
* Target solution length
* Acceptable solution range
* Other generation constraints

For example, the generator might be asked to produce a level whose solution falls within a particular push range. The generator then creates candidates and tests them until it finds one that fits the profile. This makes difficulty something that can be **measured**, rather than simply guessed.

## The Solver Is Part of the Generator

This is one of the most important pieces of the system. After a candidate level is generated, the game needs to determine whether it can actually be solved. That requires a solver.

The solver explores possible player and crate states and searches for a sequence of valid pushes that eventually places the crates in their required goal positions.

But Number Push adds another layer to that search. A push doesn't necessarily move a crate one space. The crate's number determines its movement distance. That means the solver has to account for the movement rules of Number Push when exploring possible states.

A solution that works for traditional Sokoban rules isn't necessarily a valid Number Push solution. The solver therefore becomes an important part of both the game and the level generator.

## Why Minimum Pushes Matter

Suppose the generator produces two levels. The first can be solved in 8 pushes. The second requires 25 pushes. Even if both boards are approximately the same size and contain the same number of crates, the second level is likely to provide a substantially different challenge.

The solver therefore becomes more than a way to determine whether a level works. It becomes a **measurement tool**.

The generator can say, in effect:

> I need a level in this difficulty range.

It can then keep generating and testing candidates until it finds one that satisfies that requirement. This gives the generator a measurable target rather than relying entirely on subjective judgments about difficulty.

## Rejecting Bad Levels

Procedural generation inevitably creates bad candidates. That is expected.

A candidate might be:

* Completely unsolvable
* Solvable but too easy
* Solvable but too difficult
* Structurally uninteresting
* Outside the requested board constraints
* Technically valid but containing undesirable crate or wall arrangements

Rather than trying to prevent every bad possibility during generation, Number Push can generate a candidate and then evaluate it. If it fails validation, it gets discarded.

This creates a simple but powerful loop:

**Generate → Solve → Measure → Accept or Reject**

That loop is the foundation of the system.

## Avoiding Deadlocks

Sokoban-style puzzles introduce another problem that makes procedural generation particularly challenging: deadlocks. A deadlock occurs when a crate reaches a position from which it can no longer contribute to a solution. The traditional example is pushing a crate into a non-goal corner.

Number Push introduces additional possibilities because crates can move multiple spaces in a single push. A crate can potentially end up in a position that prevents another crate from reaching its destination. A movement value can also make certain positions particularly difficult or impossible to recover from.

The player therefore has to think several pushes ahead. The generator has to account for the same thing.

This is one reason why simply checking whether the board "looks right" isn't enough. The solver provides the final answer. If the generated level cannot be solved, the generator rejects it.

## Generated Levels Can Be Tested Automatically

This is where procedural generation becomes especially useful for a game developer. Once the generation process is automated, I don't have to manually test every possible level. The generator can produce large numbers of candidates and run them through the validation process.

For example:

**Candidate 1**

Generated → Unsolvable → Rejected

**Candidate 2**

Generated → Solvable → 9 pushes → Too easy → Rejected

**Candidate 3**

Generated → Solvable → 21 pushes → Within target range → Accepted

That process can continue automatically. The result is a level-generation system that can produce content at a much larger scale than manually designing every puzzle.

## Seeds Make Procedural Generation Reproducible

Random generation creates another interesting problem. If the generator produces a great puzzle today, how do I reproduce it tomorrow? This is where **random seeds** become important.

Instead of relying on completely unpredictable randomness, the generator can use a specific seed to initialize its random-number generation. The same seed produces the same sequence of random decisions. That means the same seed can reproduce the same generated level.

This has several important advantages. A developer can record a seed when a particularly good level is discovered. A player can share a seed with another player. A daily challenge can be associated with a specific seed. A level can be stored without having to store the entire generated board. And a future version of the game can potentially recreate previously generated content.

## Seeds Also Enable a Content Pipeline

This is where the technology starts becoming particularly interesting from a game-development perspective. Imagine generating 10,000 candidate levels. The generator solves all of them. Levels outside the desired difficulty range are rejected. Additional quality checks remove undesirable candidates.

The remaining levels can be associated with their seeds. Now there is a collection of reproducible puzzle content. The game doesn't need to treat every generated level as disposable random content. It can preserve the best results.

That creates a bridge between **procedural generation and curated content**. The computer does the heavy lifting. The final game can still control which levels actually reach players.

## The Free Version

The initial version of Number Push will be available as a **free game**. The purpose is twofold. First, it should be a fun puzzle game. Second, it provides a real-world demonstration of the level-generation technology.

Rather than simply describing a procedural generator on a developer website, Number Push provides an actual application where the technology is being used to create playable content. That makes the project a practical test of the approach.

## The Pro Version

A Pro version is planned as a way to take the system further. One of the planned Pro features is access to the **seed-generation system**. This provides a more direct way to work with reproducible procedural content.

The Pro version can also become the foundation for a subscription model built around regularly updated puzzle content. Instead of selling a game that stops changing after the player finishes its original levels, the system can support an ongoing stream of new generated puzzles.

## A Subscription Built Around New Content

A subscription only makes sense when there is continuing value for the player. For Number Push, that value could come from regularly updated levels.

The generation system can continually produce candidate puzzles. Those puzzles can be solved, measured, filtered, and selected. Seeds can then be used to preserve the selected levels. New puzzle collections could be released on a regular schedule.

This creates a potential content pipeline:

**Generate → Validate → Measure → Select → Seed → Publish**

The technology makes it possible to create new content without requiring every puzzle to be manually designed from the ground up.

## Why This Is Interesting to Developers

The interesting part of Number Push isn't that it uses random numbers to create boards. That would be easy. The interesting part is building a system that can take randomness and turn it into **controlled, reproducible, measurable game content**.

There are several independent pieces working together:

**Procedural Generation**

Creates candidate boards based on defined rules.

**Constraint Checking**

Makes sure generated boards meet structural requirements.

**Puzzle Solving**

Determines whether the candidate can actually be completed using the game's movement rules.

**Difficulty Measurement**

Uses the solution to determine how challenging the level is.

**Seeded Randomness**

Allows accepted levels to be reproduced.

**Content Selection**

Allows generated levels to become part of a curated collection.

Together, these components turn procedural generation into something much more useful than simply making random content.

## The Technology Is the Real Experiment

Number Push is ultimately an experiment in how far this approach can be taken.

Can a generator consistently create interesting puzzles? Can difficulty be controlled well enough to create distinct progression? Can automated solving eliminate most unusable levels? Can the solver accurately evaluate puzzles where crates move multiple spaces per push? Can seeds provide reliable reproducibility? Can generated content support a long-term update system?

Those are more interesting questions to me than simply asking how many puzzles the game can contain.

## What I'm Building

Number Push is the visible game. Behind it is a procedural level-generation system designed to create puzzles, solve them, measure them, and reproduce the best results.

The free version will let players experience the game. The Pro version will expose more of the underlying technology and provide the foundation for regularly updated content.

And from a development perspective, the project provides something even more valuable: a practical test case for building a procedural content system that isn't satisfied with simply generating something random. It has to generate something **playable**.

## Final Thought

Procedural generation is often described as a way to create "infinite content." That's only half the story.

Generating infinite content is easy. Generating infinite **good** content is much harder.

Number Push is my attempt to explore that problem with a system that combines procedural generation, automated solving, difficulty analysis, and deterministic seeds.

The game is the result players will see. The generator is the experiment happening underneath it.

And if the experiment succeeds, Number Push could become more than a puzzle game. It could become a demonstration of a reusable approach to creating, testing, and continuously delivering procedural game content.