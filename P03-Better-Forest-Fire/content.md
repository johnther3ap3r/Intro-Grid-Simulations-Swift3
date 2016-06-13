---
title: "A more realistic forest simulation"
slug: better-forest-fire
---

> [info]
> You can open an emoji search popup on any OS X computer by holding `control + command` and press the `space bar`.

Now that we have the base simulation completed, let's try to implement the rest 🌲 simulations [here](http://ncase.me/simulating/)!

We won't be giving you much guidance this time around so make sure to read the rules careful and plan out your code. It might be best to work with a partner so you can catch each others mistakes!

# Forest fires cause by lightning

Read through "A Forest With Thunderbolt & Lightning, Very Very Frightening" on this [page](http://ncase.me/simulating/) and play with the simulation. In our last simulation, fire was caused by a human act (you). In this simulation, we'll simulate random fires started by lightning!

> [action]
> Before we get started, create two new functions in your `ForestFireSimulation` class as shown below:
>
```
func thunderboltAndLightning() {
>
}
>
func aTinyForest() {
>
}
```
>
> Take all the code you wrote in your `update()` method and copy it into the body of `aTinyForest()`. Once copied, remove all the code from `update()` and replace it with a single line calling `thunderboltAndLightning()`. We'll write our new simulation in `thunderboltAndLightning()`.

## The rules

Here is a summary of the rules that we're implementing:

- **Setup:**
  1. **Palette setup:** 🌲 and 🔥 should be in the palette.
  1. **Random seeding:** Create a `8x10` grid randomly populated with a `50%` chance of a 🌲 tile, or empty otherwise.
- **Update:**
  1. **Spawning trees:** An empty tile has a `0.3%` chance of becoming a 🌲.
  1. **Burning trees:** 🌲 turns into 🔥 if any of it's neighbors are 🔥. Otherwise, there is a `0.01%` chance it get's struck by lightning and turns into 🔥.
  1. **Sizzling out:** 🔥 dies out and becomes an empty tile.

The rules to this are pretty similar to `aTinyForest()` so you can probably reuse most of your code. Good luck!

> [action]
> Implement `thunderboltAndLightning()` according to the rules above!

Run your simulation for a while and watch as the forest burns down and grows back over time. Modify the growth rate of trees. How does that affect things?

# Killing trees to save trees

Read through "A Forest Where You Show Trees No Mercy" on this [page](http://ncase.me/simulating/) and play with the simulation. This time we'll be showing how killing trees can actually help stop the spread of forest fires!

> [action]
> Before we get started, create a new function in your `ForestFireSimulation` class as shown below:
>
```
func noMercy() {
>
}
>
```
>
> Replace the call to `thunderboltAndLightning()` in `update()` with a call to `noMercy()`. We'll write our new simulation in `noMercy()`.

## The rules

We'll be adding tree killers (✄) into this simulation. Here is a summary of the rules that we're implementing:

- **Setup:**
  1. **Palette setup:** 🌲, 🔥, ✄ should be in the palette.
  1. **Random seeding:** Create a `8x10` grid randomly populated with a `50%` chance of a 🌲 tile, or empty otherwise.
- **Update:**
  1. **Spawning trees:** An empty tile has a `1%` chance of becoming a 🌲.
  1. **Killing trees:** 🌲 turns into 🔥 if any of it's neighbors are 🔥. It becomes an empty cell if any of it's neighbors are ✄. Otherwise, there is a `0.01%` chance a 🌲 get's struck by lightning and turns into 🔥.
  1. **Sizzling out:** 🔥 dies out and becomes an empty tile.

  > [action]
  > Implement `noMercy()` according to the rules above! Make sure to add ✄ to your palette so you can contain the forest fires!

Run your simulation and create a line of ✄. Is it effective at stopping the spread of forest fires?

# Invasive species and fires

Read through "A Forest Where Some Trees Are, Like, Total Jerks" on this [page](http://ncase.me/simulating/) and play with the simulation. This time we'll explore how fires spread when there are trees of different species! Some species rely on fires for their survival...

> [action]
> Before we get started, create a new function in your `ForestFireSimulation` class as shown below:
>
```
func jerkTrees() {
>
}
>
```
>
> Replace the call to `noMercy()` in `update()` with a call to `jerkTrees()`. We'll write our new simulation in `jerkTrees()`.

## The rules

We'll be adding strong trees and jerk trees (🌳 and 🌱) into this simulation. Here is a summary of the rules that we're implementing:

- **Setup:**
  1. **Palette setup:** 🌳, 🌱, 🔥, ✄ should be in the palette.
  1. **Random seeding:** Create a `8x10` grid randomly populated with a `5%` chance of a 🌳 tile, a `5%` chance of a 🌱 tile, or empty otherwise.
- **Update:**
  1. **Spawning trees:** An empty tile has a `0.5%` chance of becoming a 🌳 and a `1%` chance of becoming a 🌱.
  1. **Killing strong trees:** 🌳 are invincible to 🔥 but can be choked of nutrients by 🌱! 🌳 becomes an empty cell if any of it's neighbors are ✄ or at least 4 of it's neighbors are 🌱.
  1. **Killing jerk trees:**: 🌱 turns into 🔥 if any of it's neighbors are 🔥. 🌱 becomes an empty cell if any of it's neighbors are ✄. Otherwise, there is a `0.01%` chance a 🌱 get's struck by lightning and turns into 🔥.
  1. **Sizzling out:** 🔥 dies out and becomes an empty tile.

  > [action]
  > Implement `jerkTrees()` according to the rules above! Make sure to set your palette to 🌳, 🌱, 🔥, ✄!

Run your simulation and watch it for a while. Does 🌳 or 🌱 take over? Or do they live in harmony? Do any patterns emerge?

# Wrapping up with trees

Go back to this [page](http://ncase.me/simulating/) and read through the end of it. Play with the "Peeps Gettin' Sick", "Rodent Racism", and "Cat/Dog Civil Conflict" simulations. We'll be creating our own, custom simulation on the next page!
