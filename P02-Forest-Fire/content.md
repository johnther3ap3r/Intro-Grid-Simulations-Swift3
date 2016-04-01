---
title: "Your Custom Simulation: Forest Fires!"
slug: forest-fire
---

Now that we've successfully replicated our Game of Life logic in a separate Xcode project, it's time to make our own custom Simulation, complete with new rules and emojis – one that simulates the spread of forest fires!

Here are the rules that we're going to decide on for this Simulation:

- Setup:
  1. Create a 10x10 grid randomly populated with a 50% chance of a 🌲 tile, or empty otherwise.
  2. Choose a random grid location. Place a 🔥 tile there.
- Update loop:
  1. 🔥 has a 5% chance of dying out, replacing itself with an empty tile.
  2. Otherwise, 🔥 has a 50% chance of spreading to each neighboring 🌲 tile.
  3. Any unaffected 🌲 tiles have a 5% chance to grow another 🌲 tile in a neighboring empty tile.

Sounds like fun! To begin, let's make a new file to denote that we are creating a new Simulation.

> [action]
> Click on the `Models` group in the left sidebar. With it selected, navigate to File...New File, or press ⌘N. Create a new Swift file entitled `ForestFireSimulation.swift`, and put it inside the folder `Grid-Simulations-Xcode/Grid-Simulations`. A video demonstrating this is here:
> ![ms-video](assets/new-file.mov)

Great! Now let's make a new class for our ForestFireSimulation, inherited from Simulation:

> [action]
> In `ForestFireSimulation.swift`, insert the following code:
>
```swift
public class ForestFireSimulation: Simulation {
    public override setup() {
>
    }
>
    public override update() {
>
    }
}
```

# Scene Setup

In the Game of Life, we loaded up an initial state by making use of a text-based loader, that automatically loaded the grid from a `.txt` file. In our simulation, though, we won't have a use for this, since our start grid state will be randomly generated. So instead, we will include our initialization code in the `setup()` function in our `ForestFireSimulation`.

First, let's disable the code that loads the grid state from file, and replace it with our own logic:

> [action]
> Open `GameViewController.swift`. Remember, this is where the loading of our scene happens!
>
> In line 17, you'll see a function loading up a `filepath` from a Resource path in the `mainBundle`. Comment out this line.
>
```swift
let filePath = NSBundle.mainBundle().pathForResource("map01", ofType: "txt")!
```
>
> Now, we'll replace the `GameOfLifeSimulation` initialization with our own initialization to `ForestFireSimulation`. Replace the following line, line 18, as follows.
>
```swift
let sim = ForestFireSimulation()
```
> Note that by default, an empty initializer will create a Simulation with a 0x0 grid. We will add our grid setup in the `setup()` function, later.
>
> In addition, let's change our palette array from the default hearts and squares to something more suitable – namely, 🔥 and 🌲. Replace line 20 with the following:
>
```swift
let palette: [Character?] = ["🔥", "🌲", nil, nil, nil, nil, nil, nil, nil, nil]
```

# (Pseudo) Random number generation

Now we can navigate to our `setup()` function in `ForestFireSimulation.swift` and add in our random initial grid state logic. But first – how do we generate random numbers?

Computers generally utilize an algorithm called [Pseudorandom Number Generator](https://en.wikipedia.org/wiki/Pseudorandom_number_generator) in order to generate sufficiently random numbers. Swift provides several ways to do this, and here is one:

```swift
random()
```

This will return a `Int` between values 0 and `RAND_MAX` – a global constant you can use in your code. So what if you instead want a double, between the range 0 and 1?

```swift
Float(random()) / Float(RAND_MAX)
```

Note that you have to cast both values to `Float`, since Swift will not implicitly convert `Int`s to `Float`s through mathematical operations (This point differentiates Swift from say, another language like Java).

However, because the pseudorandom algorithm is _deterministic_, it will give you the same results each time you run your app. That's not what we want!

Pseudorandom number generators often have something called a "seed" value – a single truly random number that all subsequent `random()` calls are based off. But what is a truly random seed that we can use? Let's try the current time:

```swift
let time = UInt32(NSDate().timeIntervalSinceReferenceDate)
srandom(time) // this "seeds" subsequent random() calls
```

So, you call the `srandom()` function once in your app, before you generate any `random()` numbers – and then your random numbers will be different each time you open your app! Let's go ahead and write this `srandom()` code somewhere that runs only once – the `viewDidLoad()` function in `GameViewController`:

> [action]
> In line 16 of `GameViewController` – right after the call to `super.viewDidLoad()` – add in the above code to "seed" the random number generator, using `srandom` and the current time.

Let's move on and apply this logic to our `setup()` function.

# Forest Fire setup

Our guidelines mentioned that we were to create a 10x10 grid, randomly filled with a 50% chance of 🔥. So, let's start by giving ourselves a 2D array!

From inside the `ForestFireSimulation` class, our grid variable can be accessed via `grid`. So, let's set `grid` to be a new 2D array:

> [action]
> Insert the following into the `setup()` function in `ForestFireSimulation.swift`:
>
```swift
grid = [[Character?]](count: 10, repeatedValue: [Character?](count: 10, repeatedValue: nil))
```

Now let's iterate through each tile in our 2D grid, and set a tile to 🌲 based on a 50% chance. But how do we do that?

Remember how `Float(random()) / Float(RAND_MAX)` returned a value between 0 and 1? If we compare that number against 0.5, we'll have a 50% chance of succeeding!

Go ahead and write the code to populate the tiles with 🌲. Do you remember how to iterate through elements of an array?

> [solution]
> Done? Your `setup()` function should look something like this:
>
```swift
grid = [[Character?]](count: 10, repeatedValue: [Character?](count: 10, repeatedValue: nil))
for x in 0..<10 {
    for y in 0..<10 {
        if Float(random()) / Float(RAND_MAX) < 0.5 {
            grid[x][y] = "🌲"
        }
    }
}
```

If you run your code, you should now see random 🌲 filling the tiles!

Great! Now we spawn our random 🔥 at a random tile location. Here, we learn about the modulo `%` operator:

```swift
10 % 3
```

The modulo operator, or `%`, will return the remainder of an integer division. So in this example, the above code will return a `1`.

Why is this useful? Well, say you wanted a random number between 0 and 9 – just perfect for the index position of our grid:

```swift
random() % 10
```

Since `random()` returns an `Int`, we can modulo it against 10 to return a value between 0 (inclusive) and 10 (exclusive). Now all we have to do is call this twice to get our random x and y coordinates, and set the corresponding tile to 🔥!

Let's see if you can write this code – place it at the end of the `setup()` function – right after the code you just wrote.

> [solution]
> Your code should look something like this:
>
```swift
let x = random() % 10
let y = random() % 10
grid[x][y] = "🔥"
```

Run your code again. You should now see a single 🔥 placed at a random location!

# Retrieving neighbors

Before we dive into the update loop, we'll write ourselves some helper functions that will retrieve the neighbors of each position in the grid.

First of all, we'll make ourselves a helper function, that determines if a grid position is in bounds of the grid, and returns a boolean. Let's call this function `isLegalPosition(x: Int, y: Int)`.

> [action]
> Create a new function, right below the empty `update()` function block. It should look like this:
>
```swift
func isLegalPosition(x: Int, _ y: Int) -> Bool {
}
```
> Now, insert code that return true if the given x and y coordinates are inside the `grid` bounds, and false otherwise.

<!--  -->

> [solution]
> Your code should look like this:
>
```swift
if 0 <= x && x < grid.count &&
   0 <= y && y < grid[0].count {
    return true
} else {
    return false
}
```

Next, we'll write a function `getNeighborPositions(originX: Int, _ originY: Int)`, whose function signature looks like this:

```swift
func getNeighborPositions(originX: Int, _ originY: Int) -> [(x: Int, y: Int)] {
}
```

What's this return type, you ask? The parentheses syntax represents Swift's _tuple_ type, which you can read about [here](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID329). Basically, it's a way for you to quickly house multiple variables of potentially different types in one container, without creating a class or a struct. So in this example, our function returns an _array_ of _tuples_ – containing two `Int`s, for the x and y positions.

Tuples can be instantiated just like this:

```swift
let tuple = (2, 3)
```

...and if the tuple is a _named_ tuple, as is the case in our return statement, you can access the individual elements like this:

```swift
let namedTuple = (x: 2, y: 3)
let x = namedTuple.x
let y = namedTuple.y
```

Let's try writing code that given an _origin_ location on the grid, returns all of the _legal_ grid positions neighboring it, as an array of tuples. How do you know if a grid position is legal? Call the function you just wrote – `isLegalPosition()`! Remember, you can use the `for x in a...b` syntax to traverse from values `a` and `b` (inclusively), and you can append to an array by calling `.append()`.

>[solution]
> Your code should look like this:
>
```swift
func getNeighborPositions(originX: Int, _ originY: Int) -> [(x: Int, y: Int)] {
    var neighbors: [(x: Int, y: Int)] = []
    for x in (originX - 1)...(originX + 1) {
        for y in (originY - 1)...(originY + 1) {
            if x == originX && y == originY {
                continue;
            }
            if isLegalPosition(x, y) {
                neighbors.append((x, y))
            }
        }
    }
    return neighbors
}
```

# The Update loop

Great! Now we are ready to start writing code for the fire propagation.

In the update loop, we'll start by making a new temporary array, where we'll store the results of the next step in the Simulation – just like we did in Game of Life. Unlike Game of Life, though, we'll need to have this `newGrid` be accessible from other functions, for reasons apparent later. So, we'll make it a class-wide variable in `ForestFireSimulation`.

> [action]
> Insert this code in the class `ForestFireSimulation`:
>
```swift
var newGrid: [[Character?]] = []
```
> Insert this code in `update()`:
>
```swift
newGrid = grid
for x in 0..<grid.count {
    for y in 0..<grid[x].count {
>
    }
}
grid = newGrid
```

Now, we check for the state of the tile in `grid`. If it's 🔥, we'll continue with our logic, first checking if it should be extinguished (20% chance).

> [action]
> Can you write this code? Inside the for loop, check for the value of the current tile. If it is 🔥, roll a random number between 0 and 1. If the number is less than 0.2, set the tile to `nil`. Remember to set the new value in `newGrid`, though!

<!--  -->

> [solution]
>
```swift
var newGrid = grid
for x in 0..<grid.count {
    for y in 0..<grid[x].count {
        let tile = grid[x][y]
        if tile == "🔥" {
            if Float(random()) / Float(RAND_MAX) < 0.2 {
                newGrid[x][y] = nil
            }
        }
    }
}
grid = newGrid
```

Now if the fire doesn't burn out, it should have a chance to spread to other tiles. We'll write a helper function here, since the logic for going through each of the neighbors can be tedious. We'll write a new function `spreadFire(x: Int, y: Int)` that takes in a grid position, and if it meets the right conditions, will spread a fire there.

> [action]
> Create a new function below `update`:
>
```swift
func spreadFire(x: Int, _ y: Int) {
>
}
```
> We've written code for you that checks if the tile is in a legal position. If it isn't, it will exit. Now write code that will check if the tile at the location is 🌲 or empty. If it is empty, you will roll for a 30% chance of replacing the tile with 🔥. If it is 🌲, you will roll for a 50% chance of replacing the tile with 🔥. Remember to set the new value in `newGrid`!

<!--  -->

> [solution]
> Your function should look like this:
>
```swift
func spreadFire(x: Int, _ y: Int) {
    let tile = grid[x][y]
    if tile == nil && Float(random()) / Float(RAND_MAX) < 0.3 {
        newGrid[x][y] = "🔥"
    }
    if tile == "🌲" && Float(random()) / Float(RAND_MAX) < 0.5 {
        newGrid[x][y] = "🔥"
    }
}
```

<!--  -->

> [action]
> Now if the tile we encounter in `update` is 🔥 _and_ it hasn't been extinguished, let's call this function for all of its neighbors, using the results from the `getNeighborPositions()` function we wrote!

<!--  -->

> [solution]
> Your `update` function should look like this:
>
```swift
public override func update() {
    newGrid = grid
    for x in 0..<grid.count {
        for y in 0..<grid[x].count {
            let tile = grid[x][y]
            if tile == "🔥" {
                if Float(random()) / Float(RAND_MAX) < 0.2 {
                    newGrid[x][y] = nil
                } else {
                    let neighbors = getNeighborPositions(x, y)
                    for neighbor in neighbors {
                        spreadFire(neighbor.x, neighbor.y)
                    }
                }
            }
        }
    }
    grid = newGrid
}
```

Great! We're almost there! All we have to do is write the same logic, but for the trees spreading. Remember, trees have a 5% chance of spreading more trees, but only if the neighboring tile is empty. Below is a function definition `spreadTree(x: Int, y: Int)` that you can use. Can you write this code yourself?

```swift
func spreadTree(x: Int, _ y: Int) {
}
```

> [solution]
> Once you're done, your `update` and `spreadTree` functions should look like this:
>
```swift
public override func update() {
    newGrid = grid
    for x in 0..<grid.count {
        for y in 0..<grid[x].count {
            let tile = grid[x][y]
            if tile == "🔥" {
                if Float(random()) / Float(RAND_MAX) < 0.05 {
                    newGrid[x][y] = nil
                } else {
                    let neighbors = getNeighborPositions(x, y)
                    for neighbor in neighbors {
                        spreadFire(neighbor.x, neighbor.y)
                    }
                }
            } else if tile == "🌲" {
                let neighbors = getNeighborPositions(x, y)
                for neighbor in neighbors {
                    spreadTree(neighbor.x, neighbor.y)
                }
            }
        }
    }
    grid = newGrid
}
>
func spreadTree(x: Int, _ y: Int) {
    let tile = grid[x][y]
    if tile == nil && Float(random()) / Float(RAND_MAX) < 0.05 {
        newGrid[x][y] = "🌲"
    }
}
```

Congrats! You've made yourself a working simulation of a forest fire in action. Try running it and watch the fires grow. How does the scene end? Let this be a lesson for you – be safe when playing with fire!
