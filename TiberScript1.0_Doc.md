# The Official TiberScript 1.0 Handbook

**Version:** 1.0

**Created by:** TiberiusCreator

**Inspired by:** microScript

**Special Thanks to:** gilles who created microStudio and created microScript!

---

#### TiberScript is a high-level game scripting language designed to eliminate the "spaghetti code" of traditional game development. It prioritizes **scoped logic** (blocks that clean up after themselves) and **readable intent** (code that reads like a sentence).

---

## 1. The Core Syntax

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

Use the `data` block for key-value collections. Unlike JSON, this supports comments and cleaner formatting.

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
* **`wave(time, speed)`**: Returns a sine wave value (great for floating animations).

---

## 2. Object-Oriented Programming (Archetypes)

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

> **Developer Note:** When you use `extends`, the child automatically gains all `mark()` tags from the parent!

---

## 3. The Game Flow (State Management)

TiberScript manages the game loop via **States**. Never use messy variables (`if state == "menu"`) to track where you are.

* **`transit("name")`**: Switches the engine to a new state immediately.
* **`during "name" do`**: Logic that only runs while in that specific state.

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
if key("arrowRight").held then walk() end  // Continuous
if key("space").hit then jump() end        // One-time action

```

---

## 5. Visuals & Audio (Scoped Effects)

TiberScript uses **Scoped Blocks** to apply effects. This prevents bugs where effects "leak" to other parts of the game (e.g., forgetting to turn off transparency).

### Graphics (`screen`)

* **`screen.sprite(id, x, y)`**: Draw an image resource.
* **`view x=..., y=... do`**: Creates a camera offset for everything inside.
* **`alter ... do`**: Applies transforms (scale, rotation, opacity, color).

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

You can use `alter` for audio too!

```tiberscript
alter volume=0.5, pitch=0.2 do
  sfx("underwater_explosion")
end

```

---

## 6. Physics (The Tag System)

Do not check for objects manually. Use **Tags**.

1. **`this.mark("tag")`**: Give an object a label in its `init`.
2. **`collide("tag")`**: Checks for collision. Returns the **Object** hit, or `null`.
3. **`near("tag", dist)`**: Checks for proximity.

```tiberscript
// Inside Player Update
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

To make TiberScript truly powerful, I have added built-in support for Timers and Particles.

### Timers (`schedule`)

Run a function after a delay without writing complex timer variables.

```tiberscript
// Run this function after 60 frames (1 second)
schedule(60) = function
  print("This message is delayed!")
end

```

### Particles (`emit`)

A built-in particle emitter system.

```tiberscript
// Spawns 10 "fire" particles at x, y
emit("fire", x, y, 10)

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
    this.mark("player") // Physics Tag
  end

  hurt(amount) = function
    this.hp = this.hp - amount
    sfx("ouch")
    
    // Advanced: Emit particles when hit
    emit("blood_particles", this.x, this.y, 5)
    
    // Visual feedback (Visuals + Scoping)
    alter color="red", scale=1.2 do
      this.draw()
    end
  end
end

enemy = archetype
  init() = function
    this.x = random(300)
    this.y = random(300)
    this.mark("hazard")
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
    if key("arrowRight").held then player.x += 5 end
    
    if key("s").hit then
      access "save1" as disk do
        disk.hp = player.hp
        disk.x = player.x
      end
      sfx("save_sound")
    end

    // Physics
    if collide("hazard") then
       player.hurt(10)
    end
    
    // Logic
    if player.hp > 50 then
      screen.text("Health Good", 10, 10)
    elsetry player.hp > 0 then
      screen.text("Health Low!", 10, 10, "red")
    else
      transit("gameOver")
    end
    
    // Rendering — draw all enemies with a for loop
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
    screen.text("Game Over", 150, 150)
  end
end

```
---
