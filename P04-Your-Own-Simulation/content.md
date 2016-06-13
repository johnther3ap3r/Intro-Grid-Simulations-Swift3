---
title: "Make Your Own Simulation!"
slug: make-your-own
---

In this tutorial, we've learned how to make use of two dimensional arrays, helper functions, and tuples. We programed Conway's Game of Life and a few forest fire simulations along the way. Now it's your turn – make your own awesome, original simulation!

Here are some guidelines to help you through the process:

1. Imagine any real-life or made-up scenario that you might be able to represent with a grid of cells. Will it be chickens running around, laying eggs? Sheep being chased by a wolf? Then, think about what the "players" of the simulation are – what will you want to keep track in your grid? Decide on emoji or characters for these players, and place them in your palette.
2. Think about what _states_ your Simulation should encompass. In `aTinyForest`, there were three states to each grid cell: 🔥, 🌲, and empty. Each of these states had pathways in which the cell could transition to another state. What are the different "actions" that each player in your simulation makes? How would that translate to the logic that you will write?
3. Once you've decided on an idea, make yourself a new class for your simulation – just like we did for `ForestFiresSimulation`. Comment out lines 36 & 37. Uncomment lines 47 & 47. Replace `YourSimulationClass()` with your actual class name. In your class, override and start implementing the `setup()` and `update()` functions. You will probably want to copy the helper functions from `ForestFireSimulation` over!
4. Make sure your code compiles, and hit Run! Step through and see your simulation in action to make sure that your code behaves as intended.

For quick reference, here's some quick documentation on the classes and functions you can use.

## Simulation

The Simulation class is what you will subclass your new Simulation from. It contains your `grid` variable that you will modify to update your simulation state, and `setup()` and `update()` that you can override to implement your simulation behavior. Here are some other useful functions:

- `init()` Default initializer. Sets `grid` to `[]`, or an empty array.
- `init(other: Simulation)` Initializer that takes in another Simulation as input. Copies the other simulation's `grid`.
- `init?(file: String)` Initializer that takes in a path to a text file to read the grid size and characters from. Note that in the text file, `0` denotes an empty cell. If anything goes wrong during the file parse, this function will return `nil`. Look at `map01.txt` for an example of how to create your text file.
- `setup()` The setup function. Called once, before the Simulation is loaded onto the scene.
- `update()` The update function. Called each time the timer ticks during play, or when the user clicks the step button.
- `grid` A variable of type `[[Character?]]` that stores the state of the simulation. Read and write to this variable in your `setup()` and `update()` functions to make changes to the grid. Make sure to set it up as an `8x10` grid!

## SimulationScene

The SimulationScene is what runs the whole operation – loading of Simulations, UI and touch event handling, and timer operations. Here are a few functions that you might find useful:

- `init(sim: Simulation, palette: [Character?])` Creates a SimulationScene with the given Simulation and character palette. This will call `setup()` on the Simulation.
- `play()` Starts playing the simulation when this is called.
- `pause()` Pauses the simulation when this is called.`

Have fun creating your simulation!
