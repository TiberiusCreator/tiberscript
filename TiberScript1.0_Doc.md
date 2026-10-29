# The Official TiberScript 1.0 Handbook

**Version:** 1.0

**Created by:** TiberiusCreator

**Inspired by:** microScript

**Special Thanks to:** gilles who created microStudio and created microScript!

> **This is the 1.0 Handbook, checked on 10/26/2026.** Every example in it runs on today's TiberScript engine in microStudio, and the full example at the end is played to its ending by microStudio's own tests. 1.0 is the core of the language: the 1.1 Handbook has everything 1.0 has and more, and the 1.2 Handbook is the newest version, the one microStudio runs. A program written for 1.0 still runs there as it is. To be sure it is read by the 1.0 rules, write `Tiber.version = "1.0"` on the first line of its file, or put the code between `tiberscript 1.0` and `end`; the 1.2 Handbook has both in its Section 1. The 1.0 Tutorial teaches the same things one lesson at a time.

---

#### TiberScript is a high level game scripting language designed to eliminate the "spaghetti code" of traditional game development. It prioritizes **scoped logic** (blocks that clean up after themselves) and **readable intent** (code that reads like a sentence).

---

## 1. The Core Syntax

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

### Loops (`for`)

Use `for` to repeat logic a set number of times. This is perfect for spawning multiple objects, drawing repeated elements, or processing a list.

```tiberscript
// Count up: runs for i = 1, 2, 3, 4, 5
for i = 1 to 5
  print("Lap " + i)
end

```

You can also loop over a list using `for ... in`:

```tiberscript
loot = ["sword", "shield", "potion"]

for item in loot
  print("Found: " + item)
end

```

> **Tip:** Use `for` inside a `during` block to draw or update many objects at once, like a row of enemies or a set of collectible coins.

### Data Structures (`data`)

Use the `data` block for key and value collections. Unlike JSON, this supports comments and cleaner formatting.

```tiberscript
config = data
  difficulty = "Hard"
  maxLives = 3
  // Configs can even hold arrays
  lootTable = ["sword", "shield", "potion"]
end

```

### Math Helpers

Global helpers exist to make math feel natural.

* **`random(n)`**: Returns a random integer between 1 and `n`.
* **`clamp(val, min, max)`**: Keeps a number inside a range.
* **`wave(time, amplitude)`**: Returns a sine wave value that swings between `-amplitude` and `amplitude` as `time` counts up (great for floating animations). Pass a frame counter as the time.

```tiberscript
roll = random(6)
hp = clamp(hp, 0, 100)
floatY = wave(frameCount, 4)
```

---

## 2. Object Oriented Programming (Archetypes)

The heart of TiberScript is the **Archetype**. This is your blueprint (class).

### Defining an Archetype

Use `archetype` to define it and `init()` as the constructor.

```tiberscript
enemy = archetype
  init(name) = function
    this.name = name
    this.hp = 100
  end
  
  die() = function
    print(this.name + " has fallen.")
  end
end

```

### Inheritance (`extends`)

Use `extends` to build upon existing archetypes.

```tiberscript
boss = archetype extends enemy
  init() = function
    super.init("Big Boss") // Calls the parent init
    this.hp = 5000
  end
end

```

> **Developer Note:** When you use `extends`, the child automatically gains all `mark()` tags from the parent! The parent's `init()` is what marks them, so a child with its own `init()` calls `super.init()`, as `boss` does.

---

## 3. The Game Flow (State Management)

A game has up to two functions that microStudio calls for you: **`init()`** runs once, at the start, and **`update()`** runs 60 times a second.

TiberScript manages the game loop via **States**. Never use messy variables (`if state == "menu"`) to track where you are.

* **`transit("name")`**: Switches the engine to a new state immediately.
* **`during "name" do`**: Logic that only runs while in that specific state.

```tiberscript
update() = function
  during "menu" do
    if key("space").hit then transit("game") end
  end

  during "game" do
    player.x += 2
  end
end

```

---

## 4. Input System

TiberScript distinguishes between a **Tap** (Intent) and a **Hold** (Action).

* **`.hit`**: Returns `true` *once* when pressed (Triggers: Jumping, Shooting, UI).
* **`.held`**: Returns `true` *continuously* while down (Movement, Charging).

Keys are named like `"space"`, `"enter"`, `"arrowLeft"`, `"arrowRight"`, `"arrowUp"`, `"arrowDown"`, or a letter like `"s"`.

```tiberscript
if key("arrowRight").held then walk() end // Continuous
if key("space").hit then jump() end // One time action

```

---

## 5. Visuals & Audio (Scoped Effects)

TiberScript uses **Scoped Blocks** to apply effects. This prevents bugs where effects "leak" to other parts of the game, like forgetting to turn off transparency.

### Graphics (`screen`)

The middle of the screen is `0, 0`. `x` grows to the right and `y` grows **up**. The screen is 200 units tall, and about 355 wide on a widescreen window.

* **`screen.clear(color)`**: Fill the whole screen with one color. Do it at the start of every frame.
* **`screen.sprite(id, x, y)`**: Draw an image resource: a sprite from the project's Sprites tab, like the `icon` every project has.
* **`screen.text(text, x, y, color)`**: Draw text. Leave out `x` and `y` for the middle of the screen, and the color for white.
* **`view x=..., y=... do`**: Creates a camera offset for everything inside. The point `x, y` is drawn in the middle of the screen. **`view world do`** draws at world positions, with no offset.
* **`alter ... do`**: Applies transforms (scale, rotation, opacity, color). Rotation is in degrees. A color paints shapes and text in that color and tints sprites.

```tiberscript
// Draw a ghost that is 50% transparent and red
alter opacity=0.5, color="red" do
  screen.sprite("ghost", x, y)
end

```

### Audio (`sfx` / `tune`)

* **`sfx("id")`**: Play a sound effect.
* **`tune("id")`**: Play looping music.
* **`silence("all")`**: Stop sounds.

The ids are the names in the project's Sounds and Music tabs.

You can use `alter` for audio too! Volume goes from 0 to 1, and a pitch of 1 is normal: 0.2 is very low.

```tiberscript
alter volume=0.5, pitch=0.2 do
  sfx("underwater_explosion")
end

```

---

## 6. Physics (The Tag System)

Do not check for objects manually. Use **Tags**.

1. **`this.mark("tag")`**: Give an object a label in its `init`.
2. **`collide("tag")`**: Checks for collision. Returns the **Object** hit, or `null`. Inside an archetype's method it checks `this`; anywhere else it checks `player`.
3. **`near("tag", dist)`**: Checks for proximity. Returns the nearest object with that tag within `dist`, or `null`.

Objects are 16 by 16 unless they have a `w` and an `h`.

```tiberscript
// Inside the player's update() method
coin = collide("currency") // Checks for any object marked "currency"

if coin then
  this.money += 1
  coin.vanish() // Removes the coin from the world
  sfx("ching")
end

```

---

## 7. Saving Data (The Access Block)

File I/O is handled safely to prevent corruption.

* **`access "slot" as var do ... end`**: Opens a save slot. It **automatically saves** when the block closes.
* **`exists("slot")`**: `true` once that slot has been saved.

```tiberscript
// Saving
access "slot1" as file do
  file.score = 1000
  file.level = 5
end 

// Loading
if exists("slot1") then
  access "slot1" as file do
    currentScore = file.score
  end
end

```

---

## 8. Advanced Tools!

To make TiberScript truly powerful, I have added built in support for Timers and Particles.

### Timers (`schedule`)

Run a function after a delay without writing complex timer variables.

```tiberscript
// Run this function after 60 frames (1 second)
schedule(60) = function
  print("This message is delayed!")
end

```

### Particles (`emit`)

A built in particle emitter system.

```tiberscript
// Spawns 10 "fire" particles at x, y
emit("fire", x, y, 10)

```

Particle kinds include `fire`, `smoke`, `spark`, `blood`, `ice`, `magic` and `coin`. Any other name gives white sparks.

### Error Handling (`attempt`)

Wrap risky logic in `attempt`. If anything goes wrong inside, the `recover` block runs instead of stopping the game. Here there is no level to read a name from, so `recover` runs.

```tiberscript
attempt
  levelName = levels[current].name
recover
  levelName = "Unknown"
end

```

---

## 9. Quick Reference Card

| Category | Keyword / Syntax | Description |
| --- | --- | --- |
| **Structure** | `archetype`, `data`, `init` | Defining classes and objects. |
| **Flow** | `during "state" do` | Managing game states. |
| **Logic** | `elsetry`, `random(n)` | Control flow and math. |
| **Loops** | `for i = 1 to n`, `for item in list` | Repeat logic or iterate a list. |
| **Errors** | `attempt ... recover` | Try/Catch error handling. |
| **Input** | `key("k").hit` / `.held` | Trigger vs Continuous input. |
| **Visuals** | `alter ... do`, `view` | Scoped rendering and camera. |
| **Audio** | `sfx()`, `tune()`, `silence()` | Sound management. |
| **Physics** | `mark()`, `collide()`, `vanish()` | Interaction system. |
| **System** | `access ... as ... do` | Safe file saving/loading. |
| **Advanced** | `emit`, `schedule` | Particles and Timers. |

---

## Example to get an idea of what TiberScript is:

```tiberscript
// --- 1. CONFIGURATION (Data) ---
config = data
  title = "Dungeon of Tiber"
  version = 1.0
  startHp = 100
end

// --- 2. ARCHETYPES (Classes) ---
hero = archetype
  init(name) = function
    this.name = name
    this.hp = config.startHp
    this.x = 0
    this.y = 0
    this.hurtTimer = 0
    this.mark("player") // Physics Tag
  end

  hurt(amount) = function
    this.hp = this.hp - amount
    this.hurtTimer = 30
    sfx("ouch")
    
    // Advanced: Emit particles when hit
    emit("blood", this.x, this.y, 5)
  end

  draw() = function
    if this.hurtTimer > 0 then
      // Visual feedback (Visuals + Scoping)
      alter color="red", scale=1.2 do
        screen.sprite("icon", this.x, this.y)
      end
    else
      screen.sprite("icon", this.x, this.y)
    end
  end
end

enemy = archetype
  init() = function
    // somewhere left or right of the hero, never on top of it
    this.x = 40 + random(110)
    if random(2) == 1 then this.x = -this.x end
    this.y = random(160) - 80
    this.mark("hazard")
  end

  draw() = function
    alter color="red", rotation=45 do
      screen.sprite("icon", this.x, this.y)
    end
  end
end

// --- 3. MAIN GAME LOOP ---
init() = function
  player = new hero("Adventurer")
  
  // Spawn 3 enemies using a for loop
  enemies = []
  for i = 1 to 3
    enemies.push(new enemy())
  end
  
  tune("dungeon_theme")
  transit("play")
end

update() = function
  
  // --- STATE: PLAYING ---
  during "play" do
    screen.clear("#222")
    
    // Input Handling
    if key("arrowRight").held then player.x += 2 end
    if key("arrowLeft").held then player.x -= 2 end
    if key("arrowUp").held then player.y += 2 end
    if key("arrowDown").held then player.y -= 2 end
    if player.hurtTimer > 0 then player.hurtTimer -= 1 end
    
    if key("s").hit then
      access "save1" as disk do
        disk.hp = player.hp
        disk.x = player.x
      end
      sfx("save_sound")
    end

    // Physics
    if collide("hazard") and player.hurtTimer == 0 then
       player.hurt(10)
    end
    
    // Logic
    if player.hp > 50 then
      screen.text("Health Good", 0, 85)
    elsetry player.hp > 0 then
      screen.text("Health Low!", 0, 85, "red")
    else
      transit("gameOver")
    end
    
    // Rendering, draw all enemies with a for loop
    view x=player.x, y=player.y do
      player.draw()
      for e in enemies
        e.draw()
      end
    end
  end

  // --- STATE: GAME OVER ---
  during "gameOver" do
    silence("all")
    screen.clear("black")
    screen.text("Game Over", 0, 0, "red")
    screen.text("Press SPACE to play again", 0, -30)
    if key("space").hit then init() end
  end
end

```

Move with the arrow keys, save with S. The red enemies hurt, and after a hurt the hero flashes red for half a second before the next one can land.
