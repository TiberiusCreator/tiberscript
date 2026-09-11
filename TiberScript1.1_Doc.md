# The Official TiberScript 1.1 Handbook

**Version:** 1.1

**Created by:** TiberiusCreator

**Inspired by:** microScript

**Special Thanks to:** gilles who created microStudio and created microScript!

---

## What's New in 1.1

Everything listed in this handbook now **actually runs** in the engine. Version 1.0 was a vision: 1.1 is the real thing.

- **Real parser.** Extra spaces, blank lines, and comments all work correctly.
- **Script-level variables.** `score = 0` and `score += 100` are real assignments.
- **Full expressions.** `+`, `-`, `*`, `/`, `==`, `!=`, `<`, `>`, `and`, `or`, `not` all evaluate correctly.
- **`if / elsetry / else`** works: both single-line and multi-line forms.
- **`for` loops** work. `for i = 1 to 5` runs the body 5 times.
- **`while` loops** work. `while hp > 0 do ... end` loops until false.
- **User-defined functions** work. Define `heal(amount) = function ... end` and call it anywhere.
- **`attempt / recover`** works. The engine actually executes error handling blocks.
- **Any key works** in `key("x").hit` / `.held`: not just arrows and space.
- **Mouse input**: `mouse.x`, `mouse.y`, `mouse.hit`, `mouse.held`.
- **Dynamic objects**: any object name auto-creates itself. No engine edits needed.
- **`screen.sprite()`** draws real microStudio sprite assets.
- **`alter` and `view`** are real scoped blocks: camera and opacity actually apply.
- **Physics helpers**: `gravity()`, `applyPhysics()`, `bounce()`, `distance()`.
- **Error logging**: unknown commands log instead of silently failing.
- **`debug(msg)`** prints and logs from inside scripts.
- **`Tiber.addCommand()`** adds new commands without touching the engine.
- **`Tiber.addScript()`** splits big games into named sub-scripts.

All 1.0 scripts run unchanged.

---

#### TiberScript is a high-level game scripting language designed to eliminate the "spaghetti code" of traditional game development. It prioritizes **scoped logic** (blocks that clean up after themselves) and **readable intent** (code that reads like a sentence).

---

## 1. The Core Syntax

### Comments

TiberScript supports two comment styles.

Use `//` for a single-line comment: everything after it on that line is ignored:

```tiberscript
speed = 5  // this is how fast the player moves

```

Use `/* ... */` for a block comment: great for long notes, disabling a chunk of code, or writing a header at the top of your script:

```tiberscript
/*
  Dungeon of Tiber: main game script
  Written by TiberiusCreator
  Version 1.1
*/

/*
  Temporarily disabled while testing:
  register("chest", 80, 0)
  register("key", -60, 40)
*/

```

### Functions & Definitions

TiberScript uses an assignment-based syntax for clarity. Functions are treated as data.

```tiberscript
// Defining a function
jump() = function
  print("Jump!")
end

// Defining a variable
speed = 10
name = "Hero"

```

You can define your own reusable functions anywhere in a script and call them by name. Parameters and local scope are fully supported.

```tiberscript
heal(amount) = function
  hp += amount
  sfx("heal_sound")
end

flash() = function
  alter opacity=0.5 do
    player.draw()
  end
end

during "game" do
  if key("h").hit then heal(25) end
end

```

### Local Variables

Use `local` inside a function to create a variable that only exists within that function. Without `local`, the variable is stored globally and can accidentally overwrite things elsewhere.

```tiberscript
distance(x1, y1, x2, y2) = function
  local dx = x2 - x1
  local dy = y2 - y1
  return sqrt(dx*dx + dy*dy)
end

move() = function
  local speed = 5
  player.x += speed
end

```

> **Tip:** Always use `local` inside user-defined functions for intermediate calculations. It keeps your scripts clean and prevents hard-to-find bugs.

### Return Values

Functions can send a value back to whoever called them using `return`. The function stops immediately at that line.

```tiberscript
isAlive() = function
  if hp > 0 then return true end
  return false
end

clampX(val) = function
  if val < -150 then return -150 end
  if val >  150 then return  150 end
  return val
end

during "game" do
  if not isAlive() then transit("gameOver") end
  player.x = clampX(player.x)
end

```

### String Operations

* **`+`**: Join strings together. Numbers are automatically converted.
* **`.length`**: Number of characters in a string.
* **`str.charAt(i)`**: The character at index `i`.
* **`str.indexOf("sub")`**: Returns the position of a substring, or -1 if not found.
* **`str.startsWith("prefix")`**: Returns `true` if the string begins with that prefix.

```tiberscript
name = "Hero"
level = 5

screen.text("Player: " + name + " (Lv " + level + ")", 0, 80, 18, "white")

if name.length > 10 then
  screen.text("Long name!", 0, 60, 15, "yellow")
end

if state.startsWith("level") then
  screen.text("In a level", 0, 40, 15, "cyan")
end

```

### Logic Control (`elsetry`)

We replace messy `else if` chains with the cleaner `elsetry` keyword. This creates a "waterfall" of logic that is easier to scan.

```tiberscript
if hp > 80 then
  status = "Healthy"
elsetry hp > 30 then
  status = "Injured"
else
  status = "Critical"
end

```

### Operators

TiberScript supports the following operators:

**Arithmetic:** `+`, `-`, `*`, `/`

**Assignment:** `=`, `+=`, `-=`, `*=`, `/=`

**Comparison:** `==`, `!=`, `<`, `>`, `<=`, `>=`

**Logic:** `and`, `or`, `not`

```tiberscript
// Arithmetic
damage = base * 2
score += 100

// Comparison
if player.x > 100 then transit("nextRoom") end
if hp <= 0 then transit("gameOver") end

// Logic — combine conditions
if alive and not stunned then
  player.x += speed
end

if collide("enemy") or collide("hazard") then
  transit("gameOver")
end

```

### Booleans and Null

TiberScript has three special values:

* **`true`** — a positive/on condition.
* **`false`** — a negative/off condition.
* **`null`** — means nothing, empty, or not found. `collide()` returns `null` when nothing is hit.

```tiberscript
isAlive = true
gameStarted = false

// Check for null — collide returns null if nothing was hit
local hit = collide("currency")
if hit then
  score += 100
  hit.vanish()
end

// null is falsy, so the if block only runs when something was actually hit

```

### Loops (`for` and `while`)

Use `for` to repeat logic a set number of times. Perfect for spawning multiple objects, drawing repeated elements, or processing a list.

```tiberscript
for i = 1 to 5
  print("Lap " + i)
end

```

Use `while` when you don't know how many iterations you need: it loops until the condition becomes false.

```tiberscript
while hp > 0 do
  hp -= 10
  print("HP remaining: " + hp)
end

```

The `by` keyword sets the step size. Without it the default step is 1. Use a negative value to count backwards.

```tiberscript
// Count by 2s
for i = 0 to 10 by 2
  print(i)  // 0, 2, 4, 6, 8, 10
end

// Count backwards
for i = 5 to 0 by -1
  print(i)  // 5, 4, 3, 2, 1, 0
end

```

Use `break` to exit a loop immediately, and `continue` to skip the rest of the current iteration and move to the next one.

```tiberscript
// Stop as soon as the item is found
for i = 0 to inventory.length - 1
  if inventory[i] == "key" then
    print("Found it at: " + i)
    break
  end
end

// Skip enemies that are off screen
for i = 0 to enemies.length - 1
  if enemies[i].x < -200 then continue end
  enemies[i].draw()
end

```

> **Tip:** `while` loops have a built-in 10,000 iteration guard to prevent infinite loops from freezing your game.

### Data Structures (`data`)

Use the `data` block for key-value collections.

```tiberscript
config = data
  difficulty = "Hard"
  maxLives = 3
  lootTable = ["sword", "shield", "potion"]
end

```

You can read data values anywhere in your script:

```tiberscript
lives = config.maxLives
first = config.lootTable[0]
count = config.lootTable.length

```

You can also push new values into a data array at runtime:

```tiberscript
config.lootTable.push("key")
print(config.lootTable.length)

```

### Math Helpers

* **`random(n)`**: Returns a random integer between 1 and `n`. Use `random(min, max)` for a range.
* **`clamp(val, min, max)`**: Keeps a number inside a range.
* **`wave(time, amplitude)`**: Returns a sine wave value (great for floating animations).
* **`distance(x1, y1, x2, y2)`**: Returns the distance between two points.
* **`min(a, b)`** / **`max(a, b)`** / **`abs(n)`** / **`sqrt(n)`** / **`floor(n)`** / **`ceil(n)`**

### Arrays

TiberScript supports lists of values. Use `[]` to create one.

* **`list.push(value)`**: Add a value to the end.
* **`list.length`**: Number of items in the list.
* **`list[i]`**: Read the item at index `i` (starts at 0).
* **`list.removeAt(i)`**: Remove the item at index `i`.

```tiberscript
inventory = ["sword", "shield", "potion"]

inventory.push("key")

for i = 0 to inventory.length - 1
  screen.text(inventory[i], -100, 80 - i * 20, 15, "white")
end

inventory.removeAt(0)

```

---

## 2. Object-Oriented Programming (Archetypes)

The heart of TiberScript is the **Archetype**. You can write it as `archetype` or the shorter `class` — both work identically.

### Defining an Archetype

Use `archetype` to define it and `init()` as the constructor.

```tiberscript
// Using 'archetype' (full keyword)
enemy = archetype
  init(name) = function
    this.name = name
    this.hp = 100
  end

  die() = function
    print(this.name + " has fallen.")
  end
end

// Using 'class' (shorter alias — same thing!)
enemy = class
  init(name) = function
    this.name = name
    this.hp = 100
  end

  die() = function
    print(this.name + " has fallen.")
  end
end

```

### Creating Instances (`new`)

Use `new` to create a living copy of an archetype. Each instance has its own independent values.

```tiberscript
init() = function
  player   = new hero("Adventurer")
  firstBoss = new boss()
  coin1    = new coin(50, 30)
  coin2    = new coin(-80, 0)
end

```

Every `new` call runs that archetype's `init()` with the arguments you pass. The result is a fully independent object — changing `coin1.x` does not affect `coin2.x`.

### Inheritance (`extends`)

Use `extends` to build upon existing archetypes.

```tiberscript
boss = archetype extends enemy
  init() = function
    super.init("Big Boss")
    this.hp = 5000
  end
end

```

> **Developer Note:** When you use `extends`, the child automatically gains all `mark()` tags from the parent!

---

## 3. The Game Flow (State Management)

TiberScript manages the game loop via **States**. Never use messy variables (`if state == "menu"`) to track where you are.

* **`transit("name")`**: Switches the engine to a new state immediately.
* **`during "name" do`**: Logic that only runs while in that specific state.

> **Note:** If you `transit()` to a state that has no matching `during` block, the screen will go blank. Always make sure every state you transit to has a `during` block handling it: even if it just shows a message.

```tiberscript
update() = function
  during "menu" do
    if key("space").hit then transit("game") end
  end

  during "game" do
    player.update()
  end
end

```

---

## 4. Input System

TiberScript distinguishes between a **Tap** (Intent) and a **Hold** (Action).

* **`.hit`**: Returns `true` *once* when pressed (Triggers: Jumping, Shooting, UI).
* **`.held`**: Returns `true` *continuously* while down (Movement, Charging).

```tiberscript
if key("arrowRight").held then walk() end
if key("space").hit then jump() end

```

Any key string works: not just arrow keys and space:

```tiberscript
if key("e").hit then interact() end
if key("shift").held then sprint() end
if key("escape").hit then transit("menu") end

```

### Mouse Input

* **`mouse.x`** / **`mouse.y`**: Cursor position in world space.
* **`mouse.hit`**: `true` for one frame when the left button is clicked.
* **`mouse.held`**: `true` continuously while the left button is held.

```tiberscript
if mouse.hit then
  bullet.x = player.x
  bullet.y = player.y
end

screen.fillRect(mouse.x, mouse.y, 4, 4, "white")

```

---

## 5. Visuals & Audio (Scoped Effects)

TiberScript uses **Scoped Blocks** to apply effects. This prevents bugs where effects "leak" to other parts of the game.

### Graphics (`screen`)

* **`screen.clear(color)`**: Clear the screen with a color before drawing each frame.
* **`screen.text(text)`**: Draw text centered on screen. Smart-positions itself based on current state.
* **`screen.text(text, x, y, size, color)`**: Draw text at a precise position with full control. Same function — extra parameters unlock full control.
* **`screen.sprite(id, x, y, size)`**: Draw a microStudio sprite asset.
* **`screen.fillRect(x, y, w, h, color)`**: Draw a filled rectangle.
* **`screen.fillCircle(x, y, radius, color)`**: Draw a filled circle.
* **`screen.fillRound(x, y, w, h, roundness, color)`**: Draw a rectangle with rounded corners. Roundness is 0 to 1.
* **`screen.fillTriangle(x1, y1, x2, y2, x3, y3, color)`**: Draw a filled triangle by its three corner points.
* **`screen.line(x1, y1, x2, y2, width, color)`**: Draw a line between two points.
* **`view x=N, y=N do`**: Camera offset: everything inside shifts by the camera position.
* **`alter opacity=N, scale=N do`**: Applies transforms only inside the block, then resets.

```tiberscript
screen.sprite("player_idle", player.x, player.y, 32)

view x=player.x, y=player.y do
  player.draw()
  enemy.draw()
end

alter opacity=0.5 do
  screen.sprite("ghost", x, y, 24)
end

alter opacity=0.5, scale=2 do
  screen.sprite("icon", x, y, 20)
end

```

> **Note:** `alter` and `view` reset automatically when the block ends. No cleanup needed.

### Object Draw Helpers

Every object has a `.draw()` shortcut and a `.sprite()` method:

```tiberscript
player.draw()
player.sprite("hero_walk", 32)

```

### Audio (`sfx` / `tune`)

* **`sfx("id")`**: Play a sound effect once.
* **`tune("id")`**: Play looping background music.
* **`silence("all")`**: Stop every sound and music immediately.
* **`silence("music")`**: Stop only the music, leave sound effects alone.
* **`pause("music")`**: Pause the current music track without stopping it.
* **`resume("music")`**: Resume a paused music track from where it left off.

```tiberscript
tune("dungeon_theme")

during "paused" do
  pause("music")
  if key("p").hit then
    resume("music")
    transit("game")
  end
end

during "gameOver" do
  silence("music")
  sfx("game_over_sting")
end

```

---

## 6. Dynamic Objects

You are no longer limited to the built-in `player`, `enemy`, and `goal` objects. Any name you use as an object is automatically created.

```tiberscript
coin.x = 50
coin.y = 30
coin.color = "gold"
coin.draw()

```

Use `register("name", x, y)` to explicitly place an object at a position:

```tiberscript
init() = function
  register("chest", 80, 0)
  register("key", -60, 40)
end

```

`collide()` works on dynamic objects too:

```tiberscript
if collide("chest") then
  score += 500
end

```

---

## 7. Physics & Logic

### Velocity

All objects have `vx` and `vy` velocity fields built in. Use `applyPhysics("name")` to move an object by its velocity each frame. This also applies world gravity automatically.

```tiberscript
gravity(0.5)

during "game" do
  if key("space").hit then player.vy = 8 end

  applyPhysics("player")

  if player.y < -80 then
    player.y = -80
    bounce("player", "y")
  end
end

```

### Physics Functions

* **`gravity(N)`**: Sets world gravity. Applied to `vy` each time `applyPhysics` is called.
* **`applyPhysics("name")`**: Moves the object by `vx`/`vy` and applies gravity.
* **`bounce("name", "x"/"y")`**: Reverses the named velocity axis.
* **`distance(x1, y1, x2, y2)`**: Returns the pixel distance between two points.

### The Tag System (Collision)

Do not check for objects manually. Use **Tags**.

1. **`this.mark("tag")`**: Give an object a label in its `init`.
2. **`collide("tag")`**: Checks for collision. Returns the **Object** hit, or `null`.
3. **`near("tag", dist)`**: Checks if any object with that tag is within `dist` pixels.
4. **`obj.vanish()`**: Removes the object from all collision checks. The object stops being detectable by `collide()` and `near()`.

```tiberscript
coin = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.mark("currency")
  end
end

during "game" do
  local hit = collide("currency")
  if hit then
    score += 100
    sfx("ching")
    hit.vanish()
  end

  local close = near("enemy", 60)
  if close then
    screen.text("DANGER!", 0, 70, 20, "red")
  end
end

```

---

## 8. Debugging & Error Handling

### `attempt / recover`

Wrap risky logic in `attempt`. If anything goes wrong inside, the `recover` block runs instead of crashing.

```tiberscript
attempt
  local result = riskyCalculation(0)
recover
  print("Something went wrong, using fallback")
  result = 0
end

```

### `debug(msg)`

Print a value from inside a script and add it to the error log.

```tiberscript
debug("player x is: " + player.x)
debug("score = " + score)

```

### Error Log

All errors are stored in `Tiber.errors`. You can read this from anywhere in your game to inspect what went wrong:

```tiberscript
during "game" do
  if Tiber.errors.length > 0 then
    screen.text("Error logged!", 0, 0, 20, "red")
  end
end

```

Unknown commands no longer fail silently: they are logged and printed to the console automatically.

---

## 9. Extensibility

You can add new commands to TiberScript **without touching the engine** by calling `Tiber.addCommand()` anywhere in your game before running the script.

```tiberscript
// Register a custom command called "explode"
Tiber.addCommand("explode", explodeHandler)

// Now usable in any TiberScript
during "game" do
  if collide("enemy") then
    explode(player.x, player.y)
    transit("gameOver")
  end
end

```

All custom commands are always checked before built-ins, so you can even override defaults.

---

## 10. Maintainability (Sub-Scripts)

For bigger games, register named sub-scripts with `Tiber.addScript()` and call them with `runScript("name")`. This keeps your main game code short and focused.

```tiberscript
// Register sub-scripts at the start of your game
Tiber.addScript("enemyAI", enemyAICode)
Tiber.addScript("hudDraw", hudCode)

// Then call them from your main during block
during "game" do
  runScript("enemyAI")
  runScript("hudDraw")
  player.draw()
  enemy.draw()
end

```

---

## 11. Saving Data (The Access Block)

File I/O is handled safely to prevent corruption.

* **`access "slot" as var do ... end`**: Opens a save slot. It **automatically saves** when the block closes.

```tiberscript
access "slot1" as file do
  file.score = 1000
  file.level = 5
end

if exists("slot1") then
  access "slot1" as file do
    currentScore = file.score
  end
end

```

---

## 12. Advanced Tools

### Timers (`schedule`)

Run a function after a delay without writing complex timer variables.

```tiberscript
schedule(60) = function
  print("This message is delayed by 1 second!")
end

```

### Particles (`emit`)

A built-in particle emitter system.

```tiberscript
emit("fire", x, y, 10)

```

### Tween (`tween`)

Smoothly animate any object property from its current value to a target value over a set number of frames.

```tiberscript
tween("scorePanel", "x", 0, 30)
tween("player", "scale", 2, 20)
tween("ghost", "opacity", 0, 60)

```

### Screen Shake (`shake`)

Shakes the screen for a set number of frames at a given intensity.

```tiberscript
if collide("enemy") then
  shake(5, 20)
  player.hp -= 10
end

```

`shake(intensity, frames)`: intensity is how many pixels the screen offsets, frames is how long it lasts.

### Repeat (`repeat`)

`repeat(N) do ... end` runs a block exactly N times without needing a counter variable. It is the clean version of `for` when you do not care about the index.

```tiberscript
// Spawn 5 coins at random positions
repeat(5) do
  register("coin" + coinCount, random(-150, 150), random(-80, 80))
  coinCount += 1
end

// Fire a burst of 3 bullets
repeat(3) do
  spawn("bullet")
end

// Draw 10 stars (simpler than for i = 1 to 10)
repeat(10) do
  screen.fillCircle(random(-150,150), random(-80,80), 2, "white")
end

```

`break` and `continue` work inside `repeat` just like they do in `for` and `while`.

### Every (`every`)

`every(N) do ... end` is TiberScript's legendary keyword. It runs its body automatically every N frames with zero timer variables needed. This does not exist as a first-class keyword in any other scripting language.

```tiberscript
// Spawn an enemy every 2 seconds (120 frames)
every(120) do
  register("enemy" + spawnCount, random(-150, 150), random(-80, 80))
  spawnCount += 1
end

// Flash the HUD every 30 frames when HP is low
if hp < 20 then
  every(30) do
    flash("red", 0.3, 5)
  end
end

// Fire a bullet every 10 frames while holding space
if key("space").held then
  every(10) do
    bullet.y += 5
    sfx("shoot")
  end
end

```

The counter resets automatically when the `every` block is entered each cycle, so it is completely self-contained. No `timer += 1`, no `if timer > N`, no cleanup.

### Flash (`flash`)

Flashes the entire screen a color for a brief moment.

```tiberscript
flash("red", 0.6, 15)
flash("white", 1, 20)

```

`flash(color, opacity, frames)`: the screen overlays that color at that opacity, then fades back to nothing over the given number of frames.

---

## 13. Quick Reference Card

| Category | Keyword / Syntax | Description |
| --- | --- | --- |
| **Structure** | `archetype` / `class`, `data`, `init` | Two ways to define a class — both identical. |
| **Instances** | `new ArchetypeName(args)` | Create a living copy of an archetype. |
| **Flow** | `during "state" do` | Managing game states. |
| **Logic** | `elsetry`, `random(n)` | Control flow and math. |
| **Operators** | `+ - * / == != < > and or not` | Arithmetic, comparison, and logic. |
| **Booleans** | `true`, `false`, `null` | Boolean values and empty/nothing. |
| **Loops** | `for i = 1 to n by step`, `while cond do` | Count loops with optional step, condition loops. |
| **Loop control** | `break`, `continue` | Exit a loop or skip to the next iteration. |
| **Functions** | `name(p) = function ... end` | Define and call your own functions. |
| **Local vars** | `local x = value` | Scoped variable inside a function. |
| **Return** | `return value` | Send a value back from a function. |
| **Strings** | `"a" + b`, `.length`, `.charAt(i)` | Concatenation and string operations. |
| **Arrays** | `list.push()`, `list[i]`, `.length` | Lists of values. |
| **Errors** | `attempt ... recover` | Error handling — runs recover on failure. |
| **Debug** | `debug(msg)` | Print and log a value from a script. |
| **Input** | `key("k").hit` / `.held` | Keyboard: trigger vs continuous. |
| **Mouse** | `mouse.x`, `mouse.y`, `mouse.hit` | Cursor position and click. |
| **Visuals** | `alter opacity=N, scale=N do` | Scoped opacity and scale. |
| **Camera** | `view x=N, y=N do` | Camera offset for draws inside. |
| **Screen** | `screen.clear(color)` | Clear the screen each frame. |
| **Screen text** | `screen.text(txt)`, `screen.text(t,x,y,sz,col)` | Short and full text drawing. |
| **Sprites** | `screen.sprite(id, x, y, size)` | Draw a microStudio sprite asset. |
| **Shapes** | `fillRect`, `fillCircle`, `fillRound`, `fillTriangle`, `screen.line` | Drawing shapes. |
| **Audio** | `sfx()`, `tune()`, `silence()` | Sound management. |
| **Audio control** | `pause("music")`, `resume("music")` | Pause and resume music. |
| **Comments** | `// line` or `/* block */` | Single-line and block comments. |
| **Physics** | `gravity()`, `applyPhysics()`, `bounce()` | Velocity and gravity system. |
| **Collision** | `mark()`, `collide()`, `vanish()` | Tag-based interaction system. |
| **Objects** | `register("name", x, y)` | Create a dynamic object. |
| **Extend** | `Tiber.addCommand(name, fn)` | Add a custom command. |
| **Modules** | `Tiber.addScript(name, code)` | Register a named sub-script. |
| **Run module** | `runScript("name")` | Execute a sub-script. |
| **System** | `access ... as ... do` | Safe file saving/loading. |
| **Advanced** | `emit`, `schedule` | Particles and Timers. |
| **Repeat** | `repeat(N) do ... end` | Run a block N times, no counter needed. |
| **Every** | `every(N) do ... end` | Run code automatically every N frames. |
| **Tween** | `tween(obj, prop, target, frames)` | Smoothly animate any property. |
| **Shake** | `shake(intensity, frames)` | Screen shake on impact. |
| **Flash** | `flash(color, opacity, frames)` | Full-screen color flash effect. |

---

## Example

```tiberscript
config = data
  title = "Dungeon of Tiber"
  startHp = 100
end

heal(amount) = function
  hp += amount
  if hp > config.startHp then hp = config.startHp end
  sfx("heal")
  debug("healed to: " + hp)
end

hero = archetype
  init(name) = function
    this.name = name
    this.hp = config.startHp
    this.mark("player")
  end
end

init() = function
  player = new hero("Adventurer")
  register("chest", 80, 0)
  gravity(0.4)
  tune("dungeon_theme")
  transit("play")
end

update() = function

  during "play" do
    screen.clear("#222")

    if key("arrowRight").held then player.x += 3 end
    if key("arrowLeft").held  then player.x -= 3 end
    if key("space").hit then player.vy = 8 end

    applyPhysics("player")
    if player.y < -80 then
      player.y = -80
      bounce("player", "y")
    end

    if key("h").hit then heal(25) end

    attempt
      if collide("chest") then
        score += 500
        sfx("treasure")
      end
    recover
      debug("chest collision failed")
    end

    if player.hp > 50 then
      screen.text("Healthy", 0, 85, 18, "green")
    elsetry player.hp > 0 then
      screen.text("Injured!", 0, 85, 18, "orange")
    else
      transit("gameOver")
    end

    view x=player.x, y=player.y do
      player.draw()
    end

    screen.text("Score: " + score, -150, 85, 15, "white")
  end

  during "gameOver" do
    silence("all")
    screen.clear("black")
    screen.text("Game Over", 0, 0, 40, "red")
    if key("space").hit then transit("play") end
  end

end

```
