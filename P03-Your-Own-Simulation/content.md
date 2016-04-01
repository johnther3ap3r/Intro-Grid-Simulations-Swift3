---
title: "Make Your Own Simulation!"
slug: make-your-own
---

In this tutorial, we've learned how to make use of nested arrays, helper functions, and tuples, in order to make our own simulation of a forest fire. Now it's your turn – make yourself your own awesome, original simulation!

Here are some guidelines to help you through the process:

1. Imagine any real-life or made-up scenario that you might be able to represent with a grid of cells. Will it be chickens running around, laying eggs? Sheep being chased by a wolf? Then, think about what the "players" of the simulation are – what will you want to keep track in your grid? Decide on emoji or characters for these players, and place them in your palette.
2. Think about what _states_ your Simulation should encompass. In Forest Fires, there were three states to each grid cell: Fire, Tree, or empty. Each of these states had pathways in which the cell could transition to another state (If you've completed our Robot Wars tutorial, the concept of a state machine should be familiar to you). What are the different "actions" that each player in your simulation makes? How would that translate to the logic that you will write?
3. Once you've decided on an idea, make yourself a new class for your simulation – just like we did for `ForestFiresSimulation`. Override and start implementing the `setup()` and `update()` functions – or if you want to load up your simulation from a text file, make yourself a new `.txt` file in the `Resources` group, and modify code in `GameViewController.swift` to load from that file.
4. Make sure your code compiles, and hit Run! Step through and see your simulation in action to make sure that your code behaves as intended.

Have fun creating your simulation!
