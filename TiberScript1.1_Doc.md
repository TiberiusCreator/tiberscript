# The Official TiberScript 1.1 Handbook

**Version:** 1.1

**Created by:** TiberiusCreator

**Inspired by:** microScript

**Special Thanks to:** gilles who created microStudio and created microScript!

> **This is the 1.1 Handbook, checked on 10/26/2026.** It describes TiberScript 1.1 exactly as its engine runs it, and every example in it runs; the full example at the end is played by microStudio's own tests. 1.1 is kept inside microStudio, frozen, for projects that already use it, and it is no longer offered in the language menu. The 1.2 Handbook is the next version, and it is the TiberScript every new project runs. Everything in this handbook still works there, with the rest of a full game on top: modules, controllers, the Timeline, swarms, the Forge, light, achievements and leaderboards. A 1.1 program can also be kept exactly as it is inside a 1.2 project: write `Tiber.version = "1.1"` on the first line of its file, or put the code between `tiberscript 1.1` and `end`, and 1.2 reads it by the 1.1 rules, the loop counters of Section 1 included. It builds on the 1.0 Handbook, and the 1.1 Tutorial teaches the same things one lesson at a time.

---

## What's New in 1.1

Everything listed in this handbook **actually runs** in the engine. Version 1.0 was a vision. 1.1 is the real thing, built into microStudio as one of its languages, and every example in this handbook has been run.

- **Real parser.** Extra spaces, blank lines, and comments all work correctly. A long expression can carry on to the next line after an operator, a comma, or an open bracket.
- **Script level variables.** `score = 0` and `score += 100` are real assignments.
- **Full expressions.** `+`, `-`, `*`, `/`, `%`, `==`, `!=`, `<`, `>`, `<=`, `>=`, `and`, `or`, `not` all evaluate correctly, with the precedence you expect.
- **`if / elsetry / else`** works: both single line and multiline forms.
- **`for` loops** work. `for i = 1 to 5` runs the body 5 times, `for item in list` visits every item.
- **`while` loops** work. `while hp > 0 do ... end` loops until false.
- **User defined functions** work. Define `heal(amount) = function ... end` and call it anywhere. Functions are values you can store and pass around.
- **`attempt / recover`** works. The engine actually executes error handling blocks, and `Tiber.lastError` says what went wrong.
- **Any key works** in `key("x").hit` / `.held`, not just arrows and space.
- **Mouse, touch, and gamepad input**: `mouse.x`, `mouse.y`, `mouse.hit`, `mouse.held`, and microStudio's own `touch` and `gamepad`.
- **Dynamic objects**: any object name automatically creates itself. No engine edits needed.
- **`screen.sprite()`** draws real microStudio sprite assets, and the whole of microStudio's screen is reachable: maps, outlines, gradients, fonts, transforms.
- **`alter` and `view`** are real scoped blocks: camera, opacity, scale, volume, and pitch actually apply.
- **Physics helpers**: `gravity()`, `applyPhysics()`, `bounce()`, `distance()`.
- **Error logging**: unknown commands log instead of silently failing.
- **`debug(msg)`** prints and logs from inside scripts.
- **`Tiber.addCommand()`** adds new commands without touching the engine.
- **`Tiber.addScript()`** splits big games into named sub scripts.

All 1.0 scripts that 1.1 can read run unchanged. Section 15 lists what 1.0 described that works differently now.

### Added in the last 1.1 update

These came in while 1.2 was being built, and are part of 1.1:

- `%` and `%=`, string escapes (`\"`, `\n`, `\\`), and single quoted strings.
- More string methods: `.endsWith()`, `.contains()`, `.toUpper()`, `.toLower()`, `.trim()`, `.substring()`, `.replace()`, `.split()`.
- More list methods: `.insert()`, `.pop()`, `.contains()`, `.indexOf()`, `.join()`, `.sort()`, `.reverse()`.
- `alter volume=N, pitch=N do` for sounds and music.
- `pause("music")` and `resume("music")` really hold the music and carry on where it was, and `tune()` always starts a song from the beginning.
- `key("shift")`, `key("control")` and `key("alt")` answer for either side of the keyboard.
- `emit()`, `flash()`, and `shake()` show in a game that has its own `draw()` function, and particles move with the world when it is drawn inside a `view` block.
- `screen.width` and `screen.height` follow the window when it is resized.
- `schedule()` takes any expression, `schedule(floor(delay / 2))` included.
- `screen.text(t, x, y)` without a size draws at size 16 in white, instead of not at all.
- A program with no `update()` function runs its top level `during` blocks every frame (Section 3).

### Fixed in the final 1.1 check

The last check before 1.2 went through the engine line by line against this handbook and the 1.0 texts. These are part of 1.1 now:

- **`0` counts as false** in an `if`, a `while`, `and`, `or` and `not`, as in microScript. `if mouse.left then`, `if gamepad.A then` and `if keyboard.SPACE then` work, because those are 1 or 0. `false`, `null` and `0` are the only false values.
- **`exists("slot")` is `true` or `false`.** It gave 1 or 0, and since 0 counted as true, `if exists("slot1") then` was true for a slot never saved.
- **Circles are round.** `screen.fillCircle`, `screen.outlineCircle` and every particle were drawn with a thin slice missing, because microStudio measures arcs in degrees and TiberScript passed radians.
- **Comments can hold quotes.** An apostrophe or a quote in a `//` comment (`// don't`) made the engine read everything after it as text, so a `/* */` comment further down stopped being a comment.
- **`for item in list` over a name holding nothing** runs zero times. It stopped the game.
- **Reading never changes anything.** Reading `player.target.x` while `target` is `null` gives `null`, and leaves `player.target` `null`: it used to put an empty object there, and `if player.target then` became true. Reading `list[9]` of a three item list gives `null` and leaves the list alone. Inside an `attempt`, reading a field of `null` is an error `recover` can catch.
- **A function kept in a field can be called:** `box.onHit = explode` then `box.onHit(3)`.
- **`alter color=` and `alter rotation=`** work, as 1.0 described: a color paints shapes and text and tints sprites, and rotation turns everything inside, in degrees.
- **`view world do`** draws at world positions, and a `view` with a name it does not know says so.
- **`screen.text(text, x, y, color)`**: a color in the size's place is read as the color, at size 16.
- **`min()` and `max()`** take any number of values, or one list.
- **`tween()`, `applyPhysics()` and `bounce()`** take an object as well as a name.
- **`Tiber.state`** is the current state and **`Tiber.frame`** counts frames.
- **A parent archetype can be written below its child**, and extending a name that is not an archetype says so.
- **A single line `if` with no `end`** says that is what is missing, instead of an error about the next word.
- **A project can have more than one file.** microStudio hands a project's files to the engine one at a time, and each one used to wipe out the ones before it, so only the last file's functions existed. Every file now adds to the others, and an error names the file it is in.
- **Changing code while the game runs keeps the game.** Saving a file reruns only that file's top level: functions and archetypes take the new code, objects already made use the new methods at once, and every other variable keeps its value. It used to start the program over without running `init()` again, and resizing the game window did the same.
- **`level.spawn = data` and `player.jump() = function`** set a field. They used to make a global whose name had a dot in it.
- **A warning is printed once** for each line that gives it, with a count, instead of sixty times a second from a draw call with a missing sprite or map.
- **About 2.4 times faster.** The engine used to read each line's text again every time the line ran. It now reads each line once and keeps what it understood: the frame of a platform game of six thousand lines went from 5.4 ms to 2.2 ms, and it makes a fifth of the garbage.

---

#### TiberScript is a high level game scripting language designed to eliminate the "spaghetti code" of traditional game development. It prioritizes **scoped logic** (blocks that clean up after themselves) and **readable intent** (code that reads like a sentence).

---

## 1. The Core Syntax

### Comments

TiberScript supports two comment styles.

Use `//` for a single line comment. Everything after it on that line is ignored:

```tiberscript
speed = 5 // this is how fast the player moves
```

Use `/* ... */` for a block comment. Great for long notes, disabling a chunk of code, or writing a header at the top of your script:

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

TiberScript uses an assignment based syntax for clarity. Functions are treated as data.

```tiberscript
// Defining a function
jump() = function
  print("Jump!")
end

// Defining a variable
speed = 10
name = "Hero"
```

You can define your own reusable functions anywhere in a script and call them by name. Parameters go on the name, in the parentheses.

```tiberscript
heal(amount) = function
  hp += amount
  sfx("heal_sound")
end

whiteout() = function
  alter opacity=0.5 do
    screen.fillRect(0, 0, 400, 250, "white")
  end
end

during "game" do
  if key("h").hit then heal(25) end
end
```

A function of your own needs a name of its own. One named like a built in, such as `flash()` or `shake()`, is never called, because the built in always answers to that name.

Because functions are values, you can store one in a variable or in a field and call it through that name, or hand it to something that wants a function, like `Tiber.addCommand()`:

```tiberscript
double(n) = function
  return n * 2
end

twice = double
print(twice(21)) // 42

rules = data
  score = double
end
print(rules.score(5)) // 10
```

### Local Variables

Use `local` inside a function to create a variable that only exists within that function. Without `local`, the variable is stored globally and can accidentally overwrite things elsewhere.

```tiberscript
distanceBetween(x1, y1, x2, y2) = function
  local dx = x2 - x1
  local dy = y2 - y1
  return sqrt(dx*dx + dy*dy)
end

move() = function
  local speed = 5
  player.x += speed
end
```

> **Tip:** Always use `local` inside user defined functions for intermediate calculations. It keeps your scripts clean and prevents hard to find bugs. A function's parameters are always local.

> **Watch out:** a `for` loop's counter is a global in 1.1. The loop itself still runs the right number of times, but if a function called from inside a `for i` loop runs its own `for i` loop, then after that call `i` holds the other loop's last value for the rest of that pass. Give loops that call each other different counter names. 1.2 makes loop counters belong to their loop.

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

### Strings

Text goes between double quotes, or single quotes when the text itself has double quotes in it. Inside double quotes, `\"` is a quote, `\n` is a new line, and `\\` is a backslash.

```tiberscript
line = "She said \"go\" and left.\nThen it was quiet."
title = 'The "Real" Dungeon'
```

* **`+`**: Join strings together. Numbers are automatically converted.
* **`.length`**: Number of characters in a string.
* **`str.charAt(i)`**: The character at index `i` (the first is 0).
* **`str.indexOf("sub")`**: The position of a substring, or -1 if not found.
* **`str.startsWith("prefix")`** / **`str.endsWith("suffix")`**: `true` if the string begins or ends with that text.
* **`str.contains("sub")`**: `true` if the text is anywhere in it.
* **`str.toUpper()`** / **`str.toLower()`**: The same text in capitals or small letters.
* **`str.trim()`**: The same text without spaces at either end.
* **`str.substring(from, to)`**: The part from index `from` up to, not including, `to`. Leave `to` out for the rest of the string.
* **`str.replace("old", "new")`**: Every `old` replaced with `new`.
* **`str.split(",")`**: A list of the pieces between each `,`.

```tiberscript
name = "Hero"
level = 5

screen.text("Player: " + name + " (Lv " + level + ")", 0, 80, 18, "white")

if name.length > 10 then
  screen.text("Long name!", 0, 60, 15, "yellow")
end

room = "level3"
if room.startsWith("level") then
  screen.text("In a level", 0, 40, 15, "cyan")
end

loot = "sword,shield,potion".split(",")
shout = name.toUpper() + "!"
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

A single line `if` still needs its `end`: `if hp <= 0 then transit("gameOver") end`.

### Operators

TiberScript supports the following operators:

**Arithmetic:** `+`, `-`, `*`, `/`, `%`

**Assignment:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`

**Comparison:** `==`, `!=`, `<`, `>`, `<=`, `>=`

**Logic:** `and`, `or`, `not`

`%` is the remainder of a division, and it wraps the way a game wants: `-1 % 4` is `3`, so an index that steps below zero lands at the end of a list instead of off it. `*`, `/` and `%` happen before `+` and `-`, and brackets group anything: `2 + 3 * 4` is `14`, `(2 + 3) * 4` is `20`.

```tiberscript
// Arithmetic
damage = base * 2
score += 100
frameInCycle = frame % 60

// Comparison
if player.x > 100 then transit("nextRoom") end
if hp <= 0 then transit("gameOver") end

// Logic: combine conditions
if alive and not stunned then
  player.x += speed
end

if collide("enemy") or collide("hazard") then
  transit("gameOver")
end
```

A long line can carry on to the next one after an operator, a comma, or an open bracket:

```tiberscript
total = baseScore + comboBonus +
  timeBonus
biggest = max(scoreA,
  scoreB)
```

### Booleans and Null

TiberScript has three special values:

* **`true`**: a positive or on condition.
* **`false`**: a negative or off condition.
* **`null`**: means nothing, empty, or not found. `collide()` returns `null` when nothing is hit.

In an `if`, a `while`, and with `and`, `or` and `not`, three values count as false: `false`, `null` and `0`. Everything else counts as true, an empty string and an empty list included. microStudio's own input objects say 1 for down and 0 for up, so `if mouse.left then` reads the way it looks.

```tiberscript
isAlive = true
gameStarted = false

// collide returns null if nothing was hit
local hit = collide("currency")
if hit then
  score += 100
  hit.vanish()
end

// null is falsy, so the if block only runs when something was actually hit
```

### Loops (`for` and `while`)

Use `for` to repeat logic a set number of times. Perfect for spawning multiple objects, drawing repeated elements, or processing a list. The `do` at the end of the line is optional.

```tiberscript
for i = 1 to 5
  print("Lap " + i)
end
```

Use `for ... in` to visit every item of a list:

```tiberscript
loot = ["sword", "shield", "potion"]

for item in loot do
  print("Found: " + item)
end
```

Use `while` when you don't know how many iterations you need. It loops until the condition becomes false. A `while` line always ends with `do`.

```tiberscript
while hp > 0 do
  hp -= 10
  print("HP remaining: " + hp)
end
```

The `by` keyword sets the step size. Without it the default step is 1. Use a negative value to count backwards. A `for` that counts from a bigger number to a smaller one without `by -1` runs zero times.

```tiberscript
// Count by 2s
for i = 0 to 10 by 2
  print(i) // 0, 2, 4, 6, 8, 10
end

// Count backwards
for i = 5 to 0 by -1
  print(i) // 5, 4, 3, 2, 1, 0
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
for e in enemies do
  if e.x < -200 then continue end
  e.draw()
end
```

> **Tip:** Loops have a built in guard to keep a mistake from freezing your game: a single `for` loop stops with an error after 200,000 steps, and a `while` loop after 50,000. The error names the loop's line.

### Data Structures (`data`)

Use the `data` block for key and value collections.

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

You can change a value, and push new values into a data array at runtime:

```tiberscript
config.difficulty = "Easy"
config.lootTable.push("key")
print(config.lootTable.length)
```

A `data` block holds values and lists. It cannot hold another `data` block inside it; make that one its own block and store it in a field instead.

### Math Helpers

* **`random(n)`**: A random whole number from 1 to `n`. `random(min, max)` gives one from `min` to `max`, both included.
* **`clamp(val, min, max)`**: Keeps a number inside a range.
* **`wave(time, amplitude)`**: A smooth up and down value, `amplitude * sin(time * 0.1)`, great for floating animations. Pass a frame counter as the time.
* **`distance(x1, y1, x2, y2)`**: The distance between two points.
* **`angle(x1, y1, x2, y2)`**: The direction from the first point to the second, in radians.
* **`lerp(a, b, t)`**: The value `t` of the way from `a` to `b` (0.5 is halfway).
* **`min(a, b, ...)`** / **`max(a, b, ...)`**: the smallest or largest of any number of values, or of one list: `max(scores)`.
* **`abs(n)`** / **`sqrt(n)`** / **`floor(n)`** / **`ceil(n)`** / **`round(n)`** / **`sign(n)`** / **`pow(a, b)`**
* **`sin`**, **`cos`**, **`tan`**, **`asin`**, **`acos`**, **`atan`**, **`atan2(y, x)`**: in radians. **`sind`**, **`cosd`**, **`tand`**, **`atan2d`**: the same in degrees.
* **`log(n)`** / **`exp(n)`**, and **`PI`**.

### Arrays

TiberScript supports lists of values. Use `[]` to create one. A list can hold anything, other lists included.

* **`list.push(value)`**: Add a value to the end.
* **`list.length`**: Number of items in the list.
* **`list[i]`**: Read or change the item at index `i` (starts at 0).
* **`list.removeAt(i)`**: Remove the item at index `i`.
* **`list.insert(i, value)`**: Put a value in at index `i`, moving the rest along.
* **`list.pop()`**: Remove the last item and give it back.
* **`list.contains(value)`**: `true` if the value is in the list.
* **`list.indexOf(value)`**: Where the value is, or -1.
* **`list.join(", ")`**: One string of every item, with `", "` between them.
* **`list.sort()`**: Put numbers in order, smallest first, or strings in alphabetical order.
* **`list.reverse()`**: Turn the list around.

```tiberscript
inventory = ["sword", "shield", "potion"]

inventory.push("key")

for i = 0 to inventory.length - 1
  screen.text(inventory[i], -100, 80 - i * 20, 15, "white")
end

inventory.removeAt(0)

if inventory.contains("key") then
  screen.text("Holding: " + inventory.join(", "), 0, -80, 12, "gold")
end

grid = [[1, 2], [3, 4]]
print(grid[1][0]) // 3
```

---

## 2. Object Oriented Programming (Archetypes)

The heart of TiberScript is the **Archetype**. You can write it as `archetype` or the shorter `class`. Both work identically.

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

// Using 'class' (shorter alias, same thing)
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
  grunt = new enemy("Grunt")
  guard = new enemy("Guard")
  guard.hp = 250
end
```

Every `new` call runs that archetype's `init()` with the arguments you pass. The result is a fully independent object. Changing `guard.hp` does not affect `grunt.hp`.

Give the archetype and the variable different names (`hero` and `player`). `player = new player()` would replace the archetype with one of its objects, and there would be nothing left to make a second one from.

### Inheritance (`extends`)

Use `extends` to build upon existing archetypes. The child has every method of its parent, and can replace any of them with its own. `super.name()` calls the parent's version, from `init()` or from any other method, as many generations up as there are. The parent can be written above or below the child.

```tiberscript
boss = archetype extends enemy
  init() = function
    super.init("Big Boss")
    this.hp = 5000
  end

  die() = function
    shake(6, 30)
    super.die()
  end
end
```

> **Developer Note:** When you use `extends`, the child automatically gains all `mark()` tags from the parent, because the parent's `init()` is what marks them.

---

## 3. The Game Flow (State Management)

A TiberScript game is built from up to three functions, and microStudio calls them for you:

* **`init()`**: runs once, when the game starts. Set things up here.
* **`update()`**: runs 60 times a second. Read input and move things here.
* **`draw()`**: runs after every `update()`. Draw here. It is optional: a game with no `draw()` can draw inside `update()`.

TiberScript manages the game loop via **States**. Never use messy variables (`if state == "menu"`) to track where you are.

* **`transit("name")`**: Switches the engine to a new state immediately.
* **`during "name" do`**: Logic that only runs while in that specific state.
* **`Tiber.state`**: The state the game is in, for a HUD or a debug line: `screen.text(Tiber.state, 0, -90, 10, "gray")`.

> **Note:** If you `transit()` to a state that has no matching `during` block, the screen will go blank. Always make sure every state you transit to has a `during` block handling it, even if it just shows a message.

```tiberscript
update() = function
  during "menu" do
    if key("space").hit then transit("game") end
  end

  during "game" do
    player.x += 1
  end
end
```

### Quick scripts: `during` without `update()`

For a small game you can skip the functions entirely and write the `during` blocks on their own, at the top of the script. When a program has no `update()` function, its top level `during` blocks run every frame, and everything else at the top level runs once, at the start. If nothing has called `transit()` by then, the game starts in the state of the first `during` block. The final test at the end of the Tutorial is written this way.

```tiberscript
during "title" do
  screen.clear("#111")
  screen.text("PRESS SPACE", 0, 0, 20, "white")
  if key("space").hit then transit("game") end
end

during "game" do
  screen.clear("#223")
  screen.text("Playing!", 0, 0, 20, "cyan")
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

Any key string works, not just arrow keys and space:

```tiberscript
if key("e").hit then interact() end
if key("shift").held then sprint() end
if key("escape").hit then transit("menu") end
```

Key names are not case sensitive. Letters and digits are themselves (`"a"`, `"7"`). The others are `"space"`, `"enter"`, `"escape"`, `"tab"`, `"backspace"`, `"shift"`, `"control"` (or `"ctrl"`), `"alt"`, `"arrowUp"`, `"arrowDown"`, `"arrowLeft"`, `"arrowRight"` (or `"up"`, `"down"`, `"left"`, `"right"`), and `"f1"` to `"f12"`. `"shift"`, `"control"` and `"alt"` answer for either side of the keyboard.

### Mouse Input

* **`mouse.x`** / **`mouse.y`**: Cursor position in world space.
* **`mouse.hit`**: `true` for one frame when a button is clicked.
* **`mouse.held`**: `true` continuously while a button is held.
* **`mouse.left`**, **`mouse.right`**, **`mouse.middle`**: 1 while that button is down, 0 while it is up, so `if mouse.right then` works.

```tiberscript
if mouse.hit then
  bullet.x = player.x
  bullet.y = player.y
end

screen.fillRect(mouse.x, mouse.y, 4, 4, "white")
```

### Touch and Gamepad

These are microStudio's own, the same ones every other microStudio language reads:

* **`touch.touching`** is 1 while a finger is on the screen, **`touch.x`** and **`touch.y`** are where the first one is, and **`touch.touches`** is a list of every finger, each with an `x` and a `y`.
* **`gamepad.A`**, **`gamepad.B`**, **`gamepad.X`**, **`gamepad.Y`**, **`gamepad.LB`**, **`gamepad.RB`**, **`gamepad.UP`**, **`gamepad.DOWN`**, **`gamepad.LEFT`**, **`gamepad.RIGHT`**, **`gamepad.MENU`** are 1 while held, and **`gamepad.press.A`** (and so on) is 1 on the frame it is pressed.

```tiberscript
if touch.touching and touch.x > 0 then player.x += 3 end
if gamepad.press.A then jump() end
```

---

## 5. Visuals & Audio (Scoped Effects)

TiberScript uses **Scoped Blocks** to apply effects. This prevents bugs where effects "leak" to other parts of the game.

### Where things are

The middle of the screen is `0, 0`. `x` grows to the right and `y` grows **up**. The screen is always 200 units tall, whatever its size in pixels, and as wide as its shape allows: about 355 on a widescreen window. `screen.width` and `screen.height` say exactly how much there is. Every shape is placed by its center.

### Graphics (`screen`)

* **`screen.clear(color)`**: Clear the screen with a color before drawing each frame.
* **`screen.text(text)`**: Draw text in the middle of the screen, at size 16, in white.
* **`screen.text(text, x, y, size, color)`**: Draw text at a precise position with full control. Size and color can be left out: they default to 16 and white. `screen.text(text, x, y, color)` is size 16 in that color.
* **`screen.sprite(id, x, y, size)`**: Draw a microStudio sprite asset.
* **`screen.fillRect(x, y, w, h, color)`**: Draw a filled rectangle. `screen.rect` is the same.
* **`screen.fillCircle(x, y, radius, color)`**: Draw a filled circle. `screen.circle` is the same.
* **`screen.fillRound(x, y, w, h, roundness, color)`**: Draw a rectangle with rounded corners. Roundness is 0 to 1.
* **`screen.fillTriangle(x1, y1, x2, y2, x3, y3, color)`**: Draw a filled triangle by its three corner points.
* **`screen.line(x1, y1, x2, y2, width, color)`**: Draw a line between two points.
* **`view x=N, y=N do`**: Camera offset: everything inside shifts by the camera position, so the point `x, y` is drawn in the middle of the screen. **`view world do`** draws at world positions, with no offset, even inside another `view`.
* **`alter opacity=N, scale=N, rotation=N, color=C do`**: Applies transforms only inside the block, then resets. Rotation is in degrees. A color paints every shape and text inside in that color, and tints sprites.

```tiberscript
screen.sprite("player_idle", player.x, player.y, 32)

view x=player.x, y=player.y do
  screen.fillRect(enemy.x, enemy.y, 16, 16, "red")
  player.draw()
end

alter opacity=0.5 do
  screen.sprite("ghost", x, y, 24)
end

alter opacity=0.5, scale=2 do
  screen.sprite("icon", x, y, 20)
end

// a red, spinning copy of the icon: a hit flash with no extra sprite
alter color="red", rotation=Tiber.frame * 4 do
  screen.sprite("icon", x, y, 20)
end
```

> **Note:** `alter` and `view` reset automatically when the block ends. No cleanup needed. `alter` knows `opacity`, `scale`, `rotation`, `color`, `volume` and `pitch`; anything else is logged, not ignored in silence. `rotation` adds to whatever `screen.rotate()` set, and nested `alter rotation=` blocks add up.

Colors are any color a web page understands: `"red"`, `"#FF5050"`, `"rgb(255, 80, 80)"`.

### The rest of the screen

Everything microStudio's screen can do, reachable from TiberScript and moved by `view` and `alter` like the rest:

* **Outlines:** `screen.outlineRect(x, y, w, h, color)`, `screen.outlineRound(x, y, w, h, roundness, color)`, `screen.outlineCircle(x, y, r, color)`, `screen.outlineTriangle(x1, y1, x2, y2, x3, y3, color)`, `screen.textOutline(text, x, y, size, color)`.
* **Maps and parts of sprites:** `screen.map(id, x, y, w, h)`, `screen.spritePart(id, sx, sy, sw, sh, x, y, w, h)`.
* **Gradients:** `screen.gradient(x1, y1, x2, y2, color1, color2)` and `screen.radialGradient(x, y, radius, color1, color2)` set a gradient that the next shape is filled with (draw it with no color).
* **Text:** `screen.textWidth(text, size)`, `screen.font(name)`, `screen.loadFont(name)`, `screen.fontReady(name)`.
* **Drawing state:** `screen.lineWidth(w)`, `screen.lineDash([4, 2])`, `screen.blending("additive")`, `screen.pixelated(on)`, `screen.anchor(ax, ay)`, `screen.translate(x, y)`, `screen.scale(sx, sy)`, `screen.rotate(angle)`, `screen.cursor(visible)`.

### Object Draw Helpers

Every object has a `.sprite()` method and a `.draw()` shortcut:

```tiberscript
player.sprite("hero_walk", 32)
player.draw()
```

`player.sprite(id, size)` gives the object a sprite and draws it once at the object's `x` and `y`. After that, `player.draw()` draws the same sprite wherever the object is now. An object that was never given a sprite draws nothing with `.draw()`, so give it one first, or draw it with a shape. An archetype can also have its own `draw()` method, and then `.draw()` calls that instead.

### Audio (`sfx` / `tune`)

* **`sfx("id")`**: Play a sound effect once.
* **`tune("id")`**: Play looping background music, from the beginning.
* **`silence("music")`**: Stop the music.
* **`silence("all")`**: Stop the music and any `audio.beep()` still sounding. Sound effects already playing finish on their own.
* **`pause("music")`**: Pause the current music track without stopping it.
* **`resume("music")`**: Resume a paused music track from where it left off.
* **`alter volume=N, pitch=N do`**: Every `sfx()` and `tune()` inside the block plays at that volume (0 to 1) and pitch (1 is normal, 2 is an octave up).
* **`audio.beep("...")`**: microStudio's beeper, for a sound with no sound file: `audio.beep("volume 10 duration 90 square C5 E5")`.

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

alter volume=0.5, pitch=0.8 do
  sfx("underwater_explosion")
end
```

The names are the ones in your project's Sounds and Music tabs, including everything made in microStudio's Sound Studio and Music Studio. `sfx()` of a name the project does not have plays nothing. To check first, the lists of names are in `sounds` and `music`:

```tiberscript
hasSound(name) = function
  for s in sounds do
    if s == name then return true end
  end
  return false
end
```

---

## 6. Dynamic Objects

You are not limited to archetypes. Any name you use as an object is automatically created, with an `x` and a `y` of 0.

```tiberscript
coin.x = 50
coin.y = 30
coin.color = "gold"
screen.fillCircle(coin.x, coin.y, 4, coin.color)
```

Use `register("name", x, y)` to explicitly place an object at a position. It also gives the object back, so you can keep working on it:

```tiberscript
init() = function
  chest = register("chest", 80, 0)
  chest.mark("chest")
  register("key", -60, 40)
end
```

`collide()` works on dynamic objects too, once they carry a tag:

```tiberscript
if collide("chest") then
  score += 500
end
```

---

## 7. Physics & Logic

### Velocity

All objects have `vx` and `vy` velocity fields built in. Use `applyPhysics("name")` to move an object by its velocity each frame. This also applies world gravity automatically. Since `y` grows up, gravity pulls `vy` down, and a jump sets `vy` to something positive.

```tiberscript
gravity(0.5)

during "game" do
  if key("space").hit then player.vy = 8 end

  applyPhysics("player")

  if player.y < -80 then
    player.y = -80
    player.vy = 0
  end
end
```

### Physics Functions

* **`gravity(N)`**: Sets world gravity. Taken off `vy` each time `applyPhysics` is called.
* **`applyPhysics("name")`**: Applies gravity, then moves the object by `vx`/`vy`. The object itself works as well as its name: `applyPhysics(this)`.
* **`bounce("name", "x"/"y")`**: Reverses the named velocity axis.
* **`distance(x1, y1, x2, y2)`**: Returns the distance between two points.

### The Tag System (Collision)

Do not check for objects manually. Use **Tags**.

1. **`this.mark("tag")`**: Give an object a label in its `init`, or `obj.mark("tag")` anywhere. `obj.unmark("tag")` takes it off, and `obj.tagged("tag")` asks.
2. **`collide("tag")`**: Checks for collision. Returns the **Object** hit, or `null`. Inside an archetype's method it checks for `this`; anywhere else it checks for `player`.
3. **`collide("tagA", "tagB")`**: Returns the first object tagged `tagA` that touches one tagged `tagB`. **`obj.collide("tag")`** checks for any one object.
4. **`near("tag", dist)`**: Returns the nearest object with that tag within `dist`, or `null`. **`obj.near("tag", dist)`** does the same from `obj`.
5. **`obj.vanish()`**: Removes the object from all collision checks. The object stops being detectable by `collide()` and `near()`.

Collision uses boxes: an object's `w` and `h`, or 16 by 16 when it has none.

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

Wrap risky logic in `attempt`. If anything goes wrong inside, the `recover` block runs instead of crashing. `Tiber.lastError` says what went wrong.

```tiberscript
attempt
  result = riskyCalculation(0)
recover
  print("Something went wrong: " + Tiber.lastError)
  result = 0
end
```

Outside an `attempt`, a name TiberScript does not know (a typo in a command or a method) is logged and the line carries on, so one typo cannot stop your game. Inside an `attempt` the same mistake is an error, so `recover` can catch it.

### `debug(msg)`

Print a value from inside a script and add it to the error log.

```tiberscript
debug("player x is: " + player.x)
debug("score = " + score)
```

`print(value)` prints without logging.

### Error Log

All errors are stored in `Tiber.errors`. You can read this from anywhere in your game to inspect what went wrong:

```tiberscript
during "game" do
  if Tiber.errors.length > 0 then
    screen.text("Error logged!", 0, 0, 20, "red")
  end
end
```

Unknown commands no longer fail silently. They are logged and printed to the console automatically, with their line. `Tiber.version` is `"1.1"`, `Tiber.state` is the current state, and `Tiber.frame` is how many frames the game has run.

---

## 9. Extensibility

You can add new commands to TiberScript **without touching the engine** by calling `Tiber.addCommand()` with a name and a function.

```tiberscript
explodeHandler(x, y) = function
  emit("boom", x, y, 30)
  shake(6, 20)
  sfx("explosion")
end

Tiber.addCommand("explode", explodeHandler)

// Now usable in any TiberScript
during "game" do
  if collide("enemy") then
    explode(player.x, player.y)
    transit("gameOver")
  end
end
```

All custom commands are always checked before built in functions, so you can even override defaults.

---

## 10. Maintainability (Sub Scripts)

For bigger games, register named sub scripts with `Tiber.addScript()` and call them with `runScript("name")`. A sub script is TiberScript, written as a string. It shares every variable and function with the main script.

```tiberscript
// Register sub scripts at the start of your game
Tiber.addScript("enemyAI", "moveEnemies()\ncheckHits()")
Tiber.addScript("hudDraw", "screen.text(\"Score: \" + score, -150, 85, 15, \"white\")")

// Then call them from your main during block
during "game" do
  runScript("enemyAI")
  runScript("hudDraw")
end
```

---

## 11. Saving Data (The Access Block)

File I/O is handled safely to prevent corruption.

* **`access "slot" as var do ... end`**: Opens a save slot. It **automatically saves** when the block closes.
* **`exists("slot")`**: `true` once that slot has been saved.

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

For a single value, microStudio's `storage` is there too: `storage.set("best", 9280)` and `best = storage.get("best")`.

---

## 12. Advanced Tools

### Timers (`schedule`)

Run a function after a delay without writing complex timer variables. The delay is in frames, and can be any expression.

```tiberscript
schedule(60) = function
  print("This message is delayed by 1 second!")
end

schedule(floor(waveDelay / 2)) = function
  print("Get ready...")
end
```

### Particles (`emit`)

A built in particle emitter system.

```tiberscript
emit("fire", x, y, 10)
```

`emit(kind, x, y, count)` knows these looks: `fire`, `smoke`, `spark`, `sparkle`, `dust`, `boom`, `hit`, `hurt`, `slash`, `spawn`, `levelup`, `dash`, `coin`, `heal`, `ice`, `lava`, `void`, `blood`, `slime`, `holy`, `snow`, `magic`. Some rise and some fall. Any other name gives white sparks. Particles live in the world: in a game that draws its world inside a `view` block, they move with it.

### Tween (`tween`)

Smoothly animate a property of an object from its current value to a target value over a set number of frames. The first argument is the object's name as a string, or the object itself (`tween(this, "y", 40, 20)` inside a method).

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

`shake(intensity, frames)`: intensity is how far the screen moves, frames is how long it lasts.

### Repeat (`repeat`)

`repeat(N) do ... end` runs a block exactly N times without needing a counter variable. It is the clean version of `for` when you do not care about the index.

```tiberscript
// Spawn 5 coins at random positions
repeat(5) do
  register("coin" + coinCount, random(-150, 150), random(-80, 80))
  coinCount += 1
end

// Draw 10 stars (simpler than for i = 1 to 10)
repeat(10) do
  screen.fillCircle(random(-150, 150), random(-80, 80), 2, "white")
end
```

`break` and `continue` work inside `repeat` just like they do in `for` and `while`.

### Every (`every`)

`every(N) do ... end` is TiberScript's legendary keyword. It runs its body every Nth time the game reaches it, with zero timer variables needed. Reached once a frame, that is every N frames. This does not exist as a first class keyword in any other scripting language.

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

Each `every` keeps its own count. One inside an `if` only counts the frames the `if` lets it run, which is exactly what the last example wants: shots come every 10 frames of holding space, not every 10 frames of the game.

### Flash (`flash`)

Flashes the entire screen a color for a brief moment.

```tiberscript
flash("red", 0.6, 15)
flash("white", 1, 20)
```

`flash(color, opacity, frames)`: the screen overlays that color at that opacity, then fades back to nothing over the given number of frames.

---

## 13. The Rest of microStudio

TiberScript runs inside microStudio, and reads the same things every other microStudio language reads:

* **`sprites`**, **`maps`**, **`sounds`**, **`music`**, **`assets`**: what the project has. `for name in sounds do` visits every sound's name, and `maps["level1"].get(x, y)` reads a tile.
* **`new Map(width, height, tileWidth, tileHeight)`**: a tile map made in code, with `.set(x, y, tile)` and `.get(x, y)`.
* **`system.time()`**, **`system.fps`**, **`system.language`**, **`system.update_rate`**: the clock in milliseconds, the frame rate, the language, and how many updates a second. `screen.text(system.fps + " fps", -150, 90, 10, "white")` puts a frame counter in a corner of your game.
* **The developer overlay:** press **F3** while any game runs, in the editor or in an exported game, or click the gauge button next to the camera in the run window. It shows the frame rate, how long each frame's `update()` and `draw()` took, the slow frames, the canvas size and a graph of the last few seconds, and it says when the browser, not your game, is holding the frame rate down (Low Power Mode on a Mac, for one). It is drawn on its own layer, so it never shows in a picture or a video of the game.
* **`storage.set(name, value)`** / **`storage.get(name)`**: saved values, kept between visits.
* **`touch`**, **`gamepad`**, **`keyboard`**: the raw input objects, as in Section 4.

---

## 14. Honest Limits

1.1 reads your script as text and runs it a line at a time. That makes it simple, forgiving and easy to debug, and it makes it slower than a compiled language. A platform game of six thousand lines, with five bosses and a crowd of enemies, spends about 2.2 ms of each 16.7 ms frame running its logic, which is plenty for an arcade game and not enough for a hundred thousand bullets. (It was 5.4 ms until the final 1.1 check taught the engine to remember each line once it has read it.) Keep the per frame work to what the frame needs: build lists once instead of every frame, and loop over what exists rather than over a whole map. 1.2's Forge compiles TiberScript and removes this limit.

Variables are global unless you say `local`, and so are loop counters (Section 1).

---

## 15. Coming From 1.0

1.0 is the core of this language, and the 1.0 Handbook and the 1.0 Tutorial run on this engine as written, since the final 1.1 check. Everything in them is in this handbook too. The first 1.0 texts had a few things that could not run anywhere. Each one is listed here, with what the 1.0 texts say now:

* **The workspace is microStudio's Code tab**, not a `gameCode` string. Make a project, pick TiberScript as its language, and write your game in `main`.
* **A single line `if` needs `end`.** `if key("space").hit then transit("game") end`.
* **Coordinates:** the middle of the screen is `0, 0` and `y` grows up, so text at `150, 150` is off the screen. `screen.text("Game Over", 0, 0)` is the middle.
* **`this.draw()` needs something to draw:** a `draw()` method of the archetype's own, or a sprite given with `.sprite(id, size)`.
* **Every archetype used with `new` must be defined** somewhere in the program.
* **`wave(time, amplitude)`**: the second value is how far the wave swings, not its speed.
* **`emit()` takes a kind it knows** (`"blood"`, not `"blood_particles"`), or gives white sparks.

## 16. Quick Reference Card

| Category | Keyword / Syntax | Description |
| --- | --- | --- |
| **Structure** | `archetype` / `class`, `data`, `init` | Two ways to define a class, both identical. |
| **Instances** | `new ArchetypeName(args)` | Create a living copy of an archetype. |
| **Inheritance** | `extends`, `super.method()` | Build on an archetype, call the parent's version. |
| **Program** | `init()`, `update()`, `draw()` | Start once, then update and draw every frame. |
| **Flow** | `during "state" do`, `transit("state")`, `Tiber.state` | Managing game states. |
| **Logic** | `if ... then`, `elsetry`, `else` | Control flow. |
| **Operators** | `+ - * / % == != < > <= >= and or not` | Arithmetic, comparison, and logic. |
| **Assignment** | `= += -= *= /= %=` | Set and change values. |
| **Booleans** | `true`, `false`, `null` | Boolean values and empty/nothing. `false`, `null` and `0` count as false. |
| **Loops** | `for i = 1 to n by step`, `for x in list`, `while cond do` | Count loops, list loops, condition loops. |
| **Loop control** | `break`, `continue` | Exit a loop or skip to the next iteration. |
| **Functions** | `name(p) = function ... end` | Define and call your own functions. |
| **Local vars** | `local x = value` | Scoped variable inside a function. |
| **Return** | `return value` | Send a value back from a function. |
| **Strings** | `"a" + b`, `.length`, `.charAt()`, `.indexOf()`, `.startsWith()`, `.endsWith()`, `.contains()`, `.toUpper()`, `.toLower()`, `.trim()`, `.substring()`, `.replace()`, `.split()` | Text and its tools. |
| **Arrays** | `list.push()`, `list[i]`, `.length`, `.removeAt()`, `.insert()`, `.pop()`, `.contains()`, `.indexOf()`, `.join()`, `.sort()`, `.reverse()` | Lists of values. |
| **Math** | `random`, `clamp`, `wave`, `distance`, `angle`, `lerp`, `min`, `max`, `abs`, `sqrt`, `floor`, `ceil`, `round`, `sign`, `pow`, `sin`..., `PI` | Math helpers. |
| **Errors** | `attempt ... recover`, `Tiber.lastError` | Error handling, runs recover on failure. |
| **Debug** | `debug(msg)`, `print(value)`, `Tiber.errors`, `Tiber.frame` | Print, log, and read the log. |
| **Input** | `key("k").hit` / `.held` | Keyboard: trigger vs continuous. |
| **Mouse** | `mouse.x`, `mouse.y`, `mouse.hit`, `mouse.held` | Cursor position and click. |
| **Touch and gamepad** | `touch.touching`, `touch.x`, `gamepad.A`, `gamepad.press.A` | microStudio's own input. |
| **Visuals** | `alter opacity=N, scale=N, rotation=N, color=C do` | Scoped opacity, scale, rotation and tint. |
| **Camera** | `view x=N, y=N do`, `view world do` | Camera offset for draws inside. |
| **Screen** | `screen.clear(color)`, `screen.width`, `screen.height` | Clear the screen, and its size. |
| **Screen text** | `screen.text(txt)`, `screen.text(t, x, y, size, color)` | Short and full text drawing. |
| **Sprites** | `screen.sprite(id, x, y, size)`, `obj.sprite(id, size)`, `obj.draw()` | Draw a microStudio sprite asset. |
| **Shapes** | `fillRect`, `fillCircle`, `fillRound`, `fillTriangle`, `line`, and the outlines | Drawing shapes. |
| **Audio** | `sfx()`, `tune()`, `silence()`, `audio.beep()` | Sound management. |
| **Audio control** | `pause("music")`, `resume("music")`, `alter volume=N, pitch=N do` | Pause, resume, volume and pitch. |
| **Comments** | `// line` or `/* block */` | Single line and block comments. |
| **Physics** | `gravity()`, `applyPhysics()`, `bounce()` | Velocity and gravity system. |
| **Collision** | `mark()`, `unmark()`, `collide()`, `near()`, `vanish()` | Tag based interaction system. |
| **Objects** | `register("name", x, y)` | Create a dynamic object. |
| **Extend** | `Tiber.addCommand(name, fn)` | Add a custom command. |
| **Modules** | `Tiber.addScript(name, code)` | Register a named sub script. |
| **Run module** | `runScript("name")` | Execute a sub script. |
| **System** | `access ... as ... do`, `exists()`, `storage` | Safe file saving/loading. |
| **Advanced** | `emit`, `schedule` | Particles and Timers. |
| **Repeat** | `repeat(N) do ... end` | Run a block N times, no counter needed. |
| **Every** | `every(N) do ... end` | Run code automatically every N frames. |
| **Tween** | `tween(obj, prop, target, frames)` | Smoothly animate any property. |
| **Shake** | `shake(intensity, frames)` | Screen shake on impact. |
| **Flash** | `flash(color, opacity, frames)` | Full screen color flash effect. |
| **microStudio** | `sprites`, `maps`, `sounds`, `music`, `system`, `new Map()` | The project and the machine. |

---

## Example

Walk with the arrows, jump with space, take a hit with D, heal with H, and walk into the chest. It runs as written in any TiberScript project: the sprite is the `icon` every project starts with.

```tiberscript
config = data
  title = "Dungeon of Tiber"
  startHp = 100
end

heal(amount) = function
  player.hp += amount
  if player.hp > config.startHp then player.hp = config.startHp end
  sfx("heal")
  debug("healed to: " + player.hp)
end

hero = archetype
  init(name) = function
    this.name = name
    this.hp = config.startHp
    this.x = -100
    this.y = -80
    this.mark("player")
    this.sprite("icon", 24)
  end
end

init() = function
  player = new hero("Adventurer")
  chest = register("chest", 80, -80)
  chest.mark("chest")
  score = 0
  gravity(0.4)
  tune("dungeon_theme")
  transit("play")
end

update() = function

  during "play" do
    screen.clear("#222")

    if key("arrowRight").held then player.x += 3 end
    if key("arrowLeft").held  then player.x -= 3 end
    if key("space").hit and player.y <= -80 then player.vy = 8 end

    applyPhysics("player")
    if player.y < -80 then
      player.y = -80
      player.vy = 0
    end

    if key("h").hit then heal(25) end
    if key("d").hit then player.hp -= 30 end

    attempt
      if collide("chest") then
        score += 500
        sfx("treasure")
        chest.vanish()
      end
    recover
      debug("chest collision failed: " + Tiber.lastError)
    end

    if player.hp > 50 then
      screen.text("Healthy", 0, 85, 18, "green")
    elsetry player.hp > 0 then
      screen.text("Injured!", 0, 85, 18, "orange")
    else
      transit("gameOver")
    end

    view x=player.x, y=0 do
      player.draw()
      screen.fillRect(chest.x, chest.y, 16, 12, "#C9A227")
    end

    screen.text("Score: " + score, -150, 85, 15, "white")
  end

  during "gameOver" do
    silence("all")
    screen.clear("black")
    screen.text("Game Over", 0, 0, 40, "red")
    if key("space").hit then
      player = new hero("Adventurer")
      transit("play")
    end
  end

end
```

This example is also a test: microStudio's own tests play it, check the chest scores 500, H heals, and a hit at zero hp ends the game.
