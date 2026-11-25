# TiberScript 1.1 Tutorial!

> **This is the 1.1 Tutorial, checked on 11/24/2026.** Every example runs on the TiberScript 1.1 engine, and the final test and the whole game after it are played to both of their endings by microStudio's own tests. It follows the 1.0 Tutorial. After this course, the 1.2 Tutorial teaches TiberScript 1.2, the version every new project runs. Everything you learn here still works there, and a 1.1 program can be kept exactly as it is inside a 1.2 project: write `Tiber.version = "1.1"` on the first line of its file, or put the code between `tiberscript 1.1` and `end`. The 1.1 Handbook has everything this course teaches in full.

## 1. Welcome to TiberScript 1.1

In this course, we will learn **TiberScript 1.1**.

TiberScript is designed to eliminate "spaghetti code" by using **Scoped Logic** and **Readable Intent**.

1.1 is the first version where everything in the language **actually runs**. Every keyword, loop, and function in this tutorial is real and working, every example has been run through the engine by microStudio's own tests, and the final test at the end has been played start to finish by the test suite.

If you took the 1.0 Tutorial, everything in it still works, and this course goes over it again on the way to the rest.

## 2. The Workspace

TiberScript is one of microStudio's languages.

1. **Make a project** in microStudio, and pick **TiberScript** as its language in the project's settings.
2. **Write your game in the Code tab**, in the source file called `main`.
3. **Press Run.** The game runs in the window next to your code, and changes you type show up while it runs.

microStudio's engine reads your TiberScript and turns it into a game. The sprites, maps, sounds and music you make in the other tabs are all reachable from your code by name.

## 3. Comments

Before writing code, know how to leave notes for yourself.

```tiberscript
// This is a single line comment

/*
  This is a block comment.
  Great for headers or temporarily disabling code.
*/

speed = 5 // You can comment at the end of a line too
```

## 4. Core Syntax: Functions & Variables

TiberScript uses an assignment based syntax. It treats functions as data to keep things clean.

**Standard code in other languages:**

```other
function jump() ... end
```

**TiberScript:**

```tiberscript
jump() = function
  print("Jump!")
end

speed = 10
name = "Hero"
```

It reads like a math equation. Jump equals a function.

## 5. Local Variables and Return

Use `local` inside a function to keep a variable scoped. Without `local`, the variable is stored globally and can accidentally overwrite things elsewhere.

```tiberscript
hypotenuse(a, b) = function
  local c = sqrt(a*a + b*b)
  return c
end

result = hypotenuse(3, 4)
print(result) // 5
```

> **Tip:** Always use `local` inside functions for intermediate values. It keeps your scripts clean and prevents hard to find bugs.

## 6. Booleans, Null, and Operators

TiberScript has three special values: `true`, `false`, and `null`.

```tiberscript
alive = true
gameOver = false
lastHit = null
```

In an `if`, `false`, `null` and `0` count as false, and everything else counts as true. So `if lastHit then` is false, and `if mouse.left then` is true only while the button is down, because it is 1 then and 0 otherwise.

**Operators:**

```tiberscript
score += 100
damage = base * 2
everyFourth = frame % 4 == 0

if hp <= 0 then transit("gameOver") end
if score == 999 then print("Perfect!") end

if alive and not stunned then player.x += speed end
if collide("enemy") or collide("hazard") then transit("gameOver") end
```

`%` is the remainder of a division: `7 % 3` is `1`. A single line `if` always ends with `end`.

## 7. Logic Control (`elsetry`)

We replace messy `else if` chains with the cleaner `elsetry` keyword.

```tiberscript
if hp > 80 then
  status = "Healthy"
elsetry hp > 30 then
  status = "Injured"
else
  status = "Critical"
end
```

## 8. Loops

### For loops

```tiberscript
for i = 1 to 3
  print("Enemy " + i + " spawned!")
end
```

Use the `by` keyword to set the step size. The default is 1.

```tiberscript
for i = 0 to 10 by 2
  print(i) // 0, 2, 4, 6, 8, 10
end

for i = 5 to 0 by -1
  print(i) // 5, 4, 3, 2, 1, 0
end
```

### For each item in a list

```tiberscript
items = ["sword", "shield", "potion"]

for item in items
  print("You found: " + item)
end
```

### While loops

A `while` line always ends with `do`.

```tiberscript
while hp > 0 do
  hp -= 10
  print("HP: " + hp)
end
```

### Break and Continue

```tiberscript
for i = 0 to 10
  if i == 5 then break end
  if i == 2 then continue end
  print(i) // 0, 1, 3, 4
end
```

### Repeat

`repeat(N) do ... end` runs a block exactly N times. No counter variable needed.

```tiberscript
coinCount = 0
repeat(5) do
  register("coin" + coinCount, random(-150, 150), random(-80, 80))
  coinCount += 1
end
```

## 9. Strings and Arrays

### Strings

```tiberscript
name = "Hero"
level = 5

screen.text("Player: " + name + " (Lv " + level + ")", 0, 80, 18, "white")

if name.length > 10 then
  print("Long name!")
end
```

A quote inside a string is `\"`, a new line is `\n`. Or use single quotes around text that has double quotes in it: `'The "Real" Dungeon'`.

### Arrays

```tiberscript
inventory = ["sword", "shield", "potion"]

inventory.push("key")

for i = 0 to inventory.length - 1
  screen.text(inventory[i], -100, 80 - i * 20, 15, "white")
end

inventory.removeAt(0)

if inventory.contains("key") then print("You can open the door.") end
print(inventory.join(", ")) // shield, potion, key
```

Lists also have `.insert(i, value)`, `.pop()`, `.indexOf(value)`, `.sort()` and `.reverse()`.

## 10. String Operations

TiberScript strings have useful built in tools.

```tiberscript
name = "Hero"

print(name.length) // 4
print(name.charAt(0)) // "H"
print(name.indexOf("e")) // 1
print(name.startsWith("He")) // true
print(name.toUpper()) // "HERO"

parts = "sword,shield,potion".split(",")
print(parts[1]) // "shield"

// Concatenation: numbers convert automatically
level = 5
screen.text("Player: " + name + " Lv " + level, 0, 80, 18, "white")
```

There are also `.endsWith()`, `.contains()`, `.toLower()`, `.trim()`, `.substring(from, to)` and `.replace(old, new)`.

## 11. Data Blocks

Use `data` to group related values together into one named collection.

```tiberscript
config = data
  title = "Dungeon of Tiber"
  startHp = 100
  speed = 3
  lootTable = ["sword", "shield", "potion"]
end
```

Read values from it anywhere:

```tiberscript
player.hp = config.startHp
print(config.lootTable[0]) // "sword"
config.lootTable.push("key")
```

## 12. Game Flow (States)

This is the most important part of TiberScript. Never use variables like `if state == "menu"` to track where you are.

Use **`during`** blocks.

```tiberscript
during "menu" do
  if key("space").hit then
    player.x = -140
    player.y = 0
    transit("game")
  end
end

during "game" do
  player.x += 2
end
```

* **`transit("name")`**: Switches the engine to a new state immediately.

A small game can be nothing but `during` blocks, like this one: they run every frame, and the game starts in the first one. A bigger game puts them inside an `update()` function, which runs every frame, next to an `init()` function that runs once at the start:

```tiberscript
init() = function
  transit("menu")
end

update() = function
  during "menu" do
    if key("space").hit then transit("game") end
  end
  during "game" do
    player.x += 2
  end
end
```

> **Note:** If you `transit()` to a state with no matching `during` block, the screen goes blank. Always cover every state you transit to.

## 13. Input: Intent vs Action

* **`.hit`**: Returns `true` once when pressed. Use for jumping, shooting, menu actions.
* **`.held`**: Returns `true` every frame while down. Use for movement.

```tiberscript
if key("arrowRight").held then player.x += 3 end
if key("space").hit then jump() end
if key("escape").hit then transit("menu") end
```

Any key string works: `"a"` to `"z"`, `"0"` to `"9"`, `"space"`, `"enter"`, `"shift"`, `"arrowUp"`, and the rest. You can also read the mouse:

```tiberscript
if mouse.hit then
  bullet.x = player.x
end

screen.fillRect(mouse.x, mouse.y, 4, 4, "white")
```

## 14. Visual Scopes

**`alter ... do`** applies effects only to the code inside the block, then resets automatically. It knows `opacity`, `scale`, `rotation` (in degrees), `color` (which tints a sprite), and, for sounds, `volume` and `pitch`.

```tiberscript
alter opacity=0.5 do
  screen.sprite("ghost", x, y)
end

alter color="red", rotation=45 do
  screen.sprite("hero", player.x, player.y)
end

// Back to normal here
screen.sprite("hero", player.x, player.y)
```

**`view x=N, y=N do`** creates a camera offset for everything inside: the point `x, y` is drawn in the middle of the screen. `view world do` draws at world positions, with no offset.

```tiberscript
view x=player.x, y=player.y do
  screen.sprite("hero", player.x, player.y)
  screen.fillRect(enemy.x, enemy.y, 16, 16, "red")
end
```

## 15. Drawing

The middle of the screen is `0, 0`, `x` grows to the right, and `y` grows **up**. The screen is 200 units tall. Shapes are placed by their center.

```tiberscript
screen.clear("#222")

screen.text("Score: " + score)
screen.text("HP: " + hp, -150, 85, 15, "red")
screen.text("LOW!", -150, 65, "red") // a color alone: size 16

screen.sprite("hero_idle", player.x, player.y, 32)

screen.fillRect(x, y, 40, 20, "cyan")
screen.fillCircle(x, y, 15, "gold")
screen.fillRound(x, y, 40, 20, 0.3, "blue")
screen.fillTriangle(x, y, x+20, y-30, x+40, y, "red")
screen.line(x1, y1, x2, y2, 2, "white")
```

An object can carry its own sprite: `player.sprite("hero_idle", 32)` gives it one, and after that `player.draw()` draws it wherever the player is. An object with no sprite draws nothing with `.draw()`.

> **Tip:** press **F3** while your game runs to see its frame rate and how long each frame takes, or read `system.fps` in your own code.

## 16. The Tag System (Collision)

1. **`this.mark("tag")`**: Label an object in its `init`, or `obj.mark("tag")` anywhere.
2. **`collide("tag")`**: Returns the object hit, or `null`.
3. **`near("tag", dist)`**: Returns the nearest tagged object within `dist`, or `null`.
4. **`obj.vanish()`**: Removes the object from all collision checks.

```tiberscript
coin = collide("currency")

if coin then
  score += 100
  sfx("ching")
  coin.vanish()
end

if near("enemy", 50) then
  screen.text("DANGER!", 0, 70, 20, "red")
end
```

`collide()` checks against `player`, or against `this` inside an archetype's own methods. An object is only found by its tags, so remember to `mark()` it.

## 17. Physics

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

* **`gravity(N)`**: Sets world gravity applied every `applyPhysics` call.
* **`applyPhysics("name")`**: Applies gravity, then moves the object by `vx`/`vy`.
* **`bounce("name", "x"/"y")`**: Reverses the named velocity axis.
* **`distance(x1, y1, x2, y2)`**: Returns the distance between two points.

```tiberscript
local d = distance(player.x, player.y, enemy.x, enemy.y)
if d < 40 then
  flash("red", 0.3, 5)
end
```

## 18. Archetypes (OOP)

We call classes "Archetypes." You can write `archetype` or the shorter `class`. Both are identical.

```tiberscript
enemy = class
  init(x, y) = function
    this.x = x
    this.y = y
    this.hp = 100
    this.mark("hazard")
  end

  die() = function
    print("Enemy down!")
    this.vanish()
  end
end

e1 = new enemy(50, 0)
e2 = new enemy(-80, 30)
```

Use `extends` to inherit from another, and `super` to call the parent:

```tiberscript
boss = class extends enemy
  init() = function
    super.init(0, 0)
    this.hp = 500
  end

  die() = function
    shake(6, 30)
    super.die()
  end
end
```

## 19. Dynamic Objects

Any name you use as an object automatically creates itself.

```tiberscript
coin.x = 50
coin.y = 30
coin.color = "gold"
screen.fillCircle(coin.x, coin.y, 4, coin.color)
```

Use `register("name", x, y)` for explicit placement. It gives the object back too:

```tiberscript
chest = register("chest", 80, 0)
chest.mark("chest")

if collide("chest") then
  score += 500
end
```

## 20. Debugging & Error Handling

```tiberscript
debug("player x: " + player.x)

attempt
  result = riskyFunction(0)
recover
  print("Something went wrong: " + Tiber.lastError)
  result = 0
end
```

Nothing fails silently. All errors are printed to the console and stored in `Tiber.errors`.

## 21. Audio

TiberScript has a full audio system.

```tiberscript
// Music
tune("dungeon_theme") // start looping music, from the beginning
pause("music") // pause it
resume("music") // resume where it left off
silence("music") // stop music only
silence("all") // stop the music and any beeps

// Sound effects
sfx("coin_pickup")
sfx("explosion")
```

Use `alter` for audio effects. Volume goes from 0 to 1, and a pitch of 1 is normal:

```tiberscript
alter volume=0.5, pitch=0.8 do
  sfx("underwater_blip")
end
```

The names are the ones in your project's Sounds and Music tabs. Sounds you make in microStudio's Sound Studio and songs from its Music Studio work straight away.

## 22. Advanced Tools

### Every

`every(N) do ... end` runs its body every N frames. Zero timer variables needed.

```tiberscript
every(120) do
  register("enemy" + spawnCount, random(-150, 150), 80)
  spawnCount += 1
end
```

### Tween, Shake, Flash

`tween()` takes an object's name, or the object itself.

```tiberscript
tween("scorePanel", "x", 0, 30)
tween(player, "y", 40, 20)

if collide("enemy") then
  shake(5, 20)
  flash("red", 0.6, 10)
end
```

## 23. Saving Data

Use `access` to safely save and load game data.

```tiberscript
// Saving
access "slot1" as file do
  file.score = score
  file.level = currentLevel
end

// Loading
if exists("slot1") then
  access "slot1" as file do
    score = file.score
    currentLevel = file.level
  end
end
```

The slot saves automatically when the `access` block closes. No manual save call needed.

## 24. Extensibility and Modules

Split big games into named sub scripts and register custom commands.

```tiberscript
// A sub script is TiberScript in a string
Tiber.addScript("enemyAI", "moveEnemies()\ncheckHits()")

// Call it from your during block
during "game" do
  runScript("enemyAI")
end
```

Register custom commands callable from TiberScript:

```tiberscript
explodeHandler(x, y) = function
  emit("boom", x, y, 30)
  shake(6, 20)
end

Tiber.addCommand("explode", explodeHandler)

during "game" do
  if collide("bomb") then
    explode(player.x, player.y)
  end
end
```

## 25. Particles and Timers

```tiberscript
// Emit 10 fire particles at x, y
emit("fire", x, y, 10)

// Run a function after 60 frames (1 second)
schedule(60) = function
  print("Delayed message!")
end
```

`emit()` knows `fire`, `smoke`, `spark`, `sparkle`, `dust`, `boom`, `hit`, `hurt`, `slash`, `spawn`, `levelup`, `dash`, `coin`, `heal`, `ice`, `lava`, `void`, `blood`, `slime`, `holy`, `snow` and `magic`.

## 26. Final Test: Dodge and Reach

Copy this into the `main` file of a new TiberScript project and press Run. Move with the arrow keys, dodge the red square as it sweeps up and down, and reach the gold one. It is nothing but `during` blocks, so it starts on the menu by itself.

```tiberscript
during "menu" do
  screen.clear("#111")
  screen.text("DODGE AND REACH", 0, 20, 24, "cyan")
  screen.text("Arrow keys to move", 0, -10, 15, "white")
  screen.text("Reach the GOLD square", 0, -30, 15, "gold")
  screen.text("Press SPACE to start", 0, -60, 14, "#aaa")
  if key("space").hit then
    register("player", -140, 0)
    register("enemy", 0, 0)
    enemy.mark("enemy")
    enemy.vy = 2
    register("goal", 140, 0)
    goal.mark("goal")
    transit("game")
  end
end

during "game" do
  screen.clear("#1a1a2e")
  screen.text("Reach the goal!", 0, 85, 13, "white")

  if key("arrowRight").held then player.x += 3 end
  if key("arrowLeft").held  then player.x -= 3 end
  if key("arrowUp").held    then player.y += 3 end
  if key("arrowDown").held  then player.y -= 3 end
  player.x = clamp(player.x, -170, 170)
  player.y = clamp(player.y, -90, 90)

  enemy.y += enemy.vy
  if enemy.y > 80 or enemy.y < -80 then bounce("enemy", "y") end

  if collide("enemy") then transit("gameOver") end
  if collide("goal")  then transit("win")      end

  screen.fillRect(player.x, player.y, 16, 16, "cyan")
  screen.fillRect(enemy.x, enemy.y, 16, 16, "red")
  screen.fillRect(goal.x, goal.y, 16, 16, "gold")
end

during "win" do
  screen.clear("#111")
  screen.text("YOU WIN!", 0, 20, 32, "gold")
  screen.text("Press SPACE to play again", 0, -20, 15, "white")
  if key("space").hit then transit("menu") end
end

during "gameOver" do
  screen.clear("#111")
  screen.text("GAME OVER", 0, 20, 30, "red")
  screen.text("Press SPACE to retry", 0, -20, 15, "white")
  if key("space").hit then transit("menu") end
end
```

Pressing SPACE on the menu places the three squares with `register()` and gives the enemy and the goal their tags with `mark()`, which is what lets `collide()` find them. `bounce()` turns the enemy around at the top and bottom, so it never stops sweeping.

## 27. The Whole Game: Tiberian Odyssey 1.1

The 1.0 course ended with Tiberian Odyssey. Here it is again, grown with what 1.1 added: the hero shoots, a hurt is followed by a moment where nothing can hurt again, each guard has a range it sees in, the Elder speaks in a box on the screen, and every sound is made by the game itself. Copy it into `main` and press Run.

**Controls:** the arrow keys (or a controller's D pad) walk, SPACE (or A) shoots the way you last walked, E (or B) talks to Elder Tim, H drinks a potion, S saves, R tries something risky, P (or MENU) pauses. On the menu, SPACE starts and L loads.

What 1.1 brings to it:

- **`class`**, the short word for `archetype`, for the bullets, the hero, the guards, the crystals and the people.
- **Shapes** in place of a sprite: `screen.fillRound`, `screen.fillRect`, `screen.fillCircle`, `screen.fillTriangle` and `screen.outlineRect`, and **`screen.text`** with a place, a size and a color.
- **`draw()`** of its own, apart from `update()`: the update moves the world, the draw shows it, and `game_state` tells the draw which screen is up.
- **`audio.beep()`** for every sound, so the game needs nothing from the Sounds tab. One function, `sound(kind)`, holds them all.
- **`shake`** and **`flash`** when the hero is hurt, **`every`** for a sparkle now and then, **`pause`** and **`resume`** for the music, **`Tiber.frame`** for the blinking and the spinning, and **`gamepad`** next to the keys.
- **Lists made in one line**, `[new enemy(1, -200, 120), ...]`, **`local`** names, and a bullet list rebuilt each frame with only the bullets still flying.

The `music` line in `game_config` is left empty. Give it the name of a song from your project's Music tab, and the game plays it.

```tiberscript
// Tiberian Odyssey 1.1: a whole game in TiberScript 1.1

game_config = data
  title = "Tiberian Odyssey 1.1"
  start_hp = 100
  speed = 2.4
  enemy_speed = 0.8
  enemy_sight = 100
  enemy_damage = 10
  safe_ticks = 60
  potions = 2
  potion_heal = 25
  crystals_needed = 5
  bullet_speed = 6
  // a song from your Music tab: left empty, the game plays without one
  music = ""
end

sound(kind) = function
  if kind == "shoot" then audio.beep("volume 6 duration 40 square C6 G5") end
  if kind == "hit" then audio.beep("volume 10 duration 90 sawtooth C3 G2") end
  if kind == "crystal" then audio.beep("volume 8 duration 60 square E5 G5 C6") end
  if kind == "defeat" then audio.beep("volume 8 duration 70 noise C4 C3") end
  if kind == "heal" then audio.beep("volume 7 duration 90 sine C5 E5 G5") end
  if kind == "save" then audio.beep("volume 6 duration 70 square G5 C6") end
  if kind == "win" then audio.beep("volume 9 duration 140 square C5 E5 G5 C6") end
  if kind == "lose" then audio.beep("volume 9 duration 180 sawtooth E3 C3 A2") end
end

say(text) = function
  dialogue_text = text
  dialogue_timer = 180
end

bullet = class
  init(x, y, dx, dy) = function
    this.x = x
    this.y = y
    this.vx = dx * game_config.bullet_speed
    this.vy = dy * game_config.bullet_speed
    this.alive = true
    this.mark("bullet")
  end

  update() = function
    this.x += this.vx
    this.y += this.vy
    if this.x < -260 or this.x > 260 or this.y < -160 or this.y > 160 then
      this.remove()
      return
    end
    local target = collide("enemy")
    if target and target.alive then
      target.defeat()
      player.score += 100
      this.remove()
    end
  end

  remove() = function
    this.alive = false
    this.vanish()
  end

  draw() = function
    screen.fillCircle(this.x, this.y, 3, "yellow")
  end
end

hero = class
  init(name) = function
    this.name = name
    this.hp = game_config.start_hp
    this.x = 0
    this.y = 0
    this.w = 16
    this.h = 16
    this.face_x = 1
    this.face_y = 0
    this.score = 0
    this.crystals = 0
    this.potions = game_config.potions
    this.safe = 0
    this.mark("player")
  end

  update() = function
    if this.safe > 0 then this.safe -= 1 end
    local dx = 0
    local dy = 0
    if key("arrowRight").held or gamepad.RIGHT then dx += 1 end
    if key("arrowLeft").held or gamepad.LEFT then dx -= 1 end
    if key("arrowUp").held or gamepad.UP then dy += 1 end
    if key("arrowDown").held or gamepad.DOWN then dy -= 1 end
    if dx != 0 or dy != 0 then
      this.face_x = dx
      this.face_y = dy
    end
    this.x = clamp(this.x + dx * game_config.speed, -230, 230)
    this.y = clamp(this.y + dy * game_config.speed, -140, 140)
    if key("space").hit or gamepad.press.A then
      bullets.push(new bullet(this.x, this.y, this.face_x, this.face_y))
      sound("shoot")
    end
  end

  hurt(amount) = function
    if this.safe > 0 then return end
    this.hp -= amount
    this.safe = game_config.safe_ticks
    sound("hit")
    shake(4, 12)
    flash("red", 0.35, 10)
    emit("blood", this.x, this.y, 6)
    if this.hp <= 0 then lose() end
  end

  heal() = function
    if this.potions == 0 then
      say("No potions left!")
    elsetry this.hp >= game_config.start_hp then
      say("Already at full health")
    else
      this.potions -= 1
      this.hp = min(this.hp + game_config.potion_heal, game_config.start_hp)
      sound("heal")
      emit("magic", this.x, this.y, 10)
      say("A potion: " + this.hp + " HP")
    end
  end

  save() = function
    access "odyssey11_save" as file do
      file.hp = this.hp
      file.x = this.x
      file.y = this.y
      file.score = this.score
      file.potions = this.potions
      file.found = found_list()
    end
    sound("save")
    say("Saved")
  end

  load() = function
    if not exists("odyssey11_save") then
      say("No save yet")
      return
    end
    access "odyssey11_save" as file do
      this.hp = file.hp
      this.x = file.x
      this.y = file.y
      this.score = file.score
      this.potions = file.potions
      for i in file.found do
        crystals[i].take(this, false)
      end
    end
    say("Welcome back, " + this.name)
  end

  draw() = function
    local color = "cyan"
    if this.safe > 0 and Tiber.frame % 10 < 5 then color = "magenta" end
    screen.fillRound(this.x, this.y, 16, 16, 0.3, color)
    screen.fillCircle(this.x + this.face_x * 9, this.y + this.face_y * 9, 2, "white")
  end
end

enemy = class
  init(id, x, y) = function
    this.id = "Enemy " + id
    this.x = x
    this.y = y
    this.w = 16
    this.h = 16
    this.alive = true
    this.mark("enemy")
  end

  update(target) = function
    if not this.alive then return end
    if distance(this.x, this.y, target.x, target.y) < game_config.enemy_sight then
      if this.x < target.x then this.x += game_config.enemy_speed end
      if this.x > target.x then this.x -= game_config.enemy_speed end
      if this.y < target.y then this.y += game_config.enemy_speed end
      if this.y > target.y then this.y -= game_config.enemy_speed end
      if collide("player") then target.hurt(game_config.enemy_damage) end
    end
  end

  defeat() = function
    this.alive = false
    sound("defeat")
    emit("spark", this.x, this.y, 15)
    this.vanish()
  end

  draw() = function
    screen.fillRect(this.x, this.y, 16, 16, "red")
    alter opacity=0.12 do
      screen.fillCircle(this.x, this.y, game_config.enemy_sight, "red")
    end
  end
end

crystal = class
  init(index, x, y) = function
    this.index = index
    this.x = x
    this.y = y
    this.taken = false
    this.mark("crystal")
  end

  take(collector, loud) = function
    if this.taken then return end
    this.taken = true
    collector.crystals += 1
    if loud then
      collector.score += 150
      sound("crystal")
      emit("coin", this.x, this.y, 8)
    end
    this.vanish()
  end

  draw() = function
    alter opacity=0.9, rotation=Tiber.frame * 2 do
      screen.fillTriangle(this.x - 6, this.y + wave(Tiber.frame + this.index * 15, 3) - 5, this.x + 6, this.y + wave(Tiber.frame + this.index * 15, 3) - 5, this.x, this.y + wave(Tiber.frame + this.index * 15, 3) + 8, "gold")
    end
  end
end

character = class
  init(name, x, y) = function
    this.name = name
    this.x = x
    this.y = y
    this.mark("npc")
  end

  talk() = function
    say(this.name + ": Hello, traveler!")
  end

  draw() = function
    screen.fillRound(this.x, this.y, 16, 18, 0.4, "#AAAAFF")
    screen.text(this.name, this.x, this.y + 16, 9, "#AAAAFF")
  end
end

elder = class extends character
  init() = function
    super.init("Elder Tim", 0, 65)
    this.asked = false
  end

  talk() = function
    if not this.asked then
      this.asked = true
      say("Elder Tim: Collect " + game_config.crystals_needed + " shards to save us!")
      schedule(600) = function
        if npcs[0].asked and game_state == "game" then say("Elder Tim: Keep searching!") end
      end
    elsetry player.crystals >= game_config.crystals_needed then
      say("Elder Tim: You restored the Harmony!")
      win()
    else
      say("Elder Tim: Need " + (game_config.crystals_needed - player.crystals) + " more crystals!")
    end
  end
end

player = null
enemies = []
crystals = []
npcs = []
bullets = []
dialogue_text = ""
dialogue_timer = 0
game_state = "menu"

found_list() = function
  local found = []
  for c in crystals do
    if c.taken then found.push(c.index) end
  end
  return found
end

clear_world() = function
  if player then player.vanish() end
  for e in enemies do
    if e.alive then e.vanish() end
  end
  for c in crystals do
    if not c.taken then c.vanish() end
  end
  for n in npcs do
    n.vanish()
  end
  for b in bullets do
    if b.alive then b.vanish() end
  end
end

new_world() = function
  clear_world()
  player = new hero("Hero")
  enemies = [new enemy(1, -200, 120), new enemy(2, 200, 120), new enemy(3, -200, -120), new enemy(4, 200, -120)]
  crystals = [new crystal(0, -100, -50), new crystal(1, 100, -50), new crystal(2, -100, 50), new crystal(3, 100, 50), new crystal(4, 0, -20)]
  npcs = [new elder()]
  bullets = []
  dialogue_text = ""
  dialogue_timer = 0
end

go(state) = function
  game_state = state
  transit(state)
end

win() = function
  silence("all")
  sound("win")
  go("win")
end

lose() = function
  silence("all")
  sound("lose")
  go("gameOver")
end

init() = function
  new_world()
  print("Welcome to " + game_config.title)
  go("menu")
end

update() = function
  during "menu" do
    if key("space").hit or gamepad.press.A then
      tune(game_config.music)
      go("game")
      return
    end
    if key("l").hit then
      player.load()
      tune(game_config.music)
      go("game")
      return
    end
  end

  during "game" do
    player.update()

    local live = []
    for b in bullets do
      if b.alive then b.update() end
      if b.alive then live.push(b) end
    end
    bullets = live

    for e in enemies do
      e.update(player)
    end

    local gem = collide("crystal")
    if gem then gem.take(player, true) end

    nearby = near("npc", 40)
    if nearby and (key("e").hit or gamepad.press.B) then nearby.talk() end

    if key("h").hit then player.heal() end
    if key("s").hit then player.save() end

    if key("r").hit then
      attempt
        if random(1, 3) == 1 then
          chest.lock.open()
        end
        player.score += 50
        say("A risk that paid: +50")
      recover
        say("The risk went wrong")
        player.hurt(20)
      end
    end

    every(300) do
      emit("magic", random(-140, 140), random(-80, 80), 2)
    end

    if dialogue_timer > 0 then dialogue_timer -= 1 end

    if key("p").hit or gamepad.press.MENU then
      pause("music")
      go("paused")
      return
    end
  end

  during "paused" do
    if key("p").hit or gamepad.press.MENU then
      resume("music")
      go("game")
      return
    end
  end

  during "gameOver" do
    if key("space").hit or gamepad.press.A then
      new_world()
      go("menu")
      return
    end
  end

  during "win" do
    if key("space").hit or gamepad.press.A then
      new_world()
      go("menu")
      return
    end
  end
end

draw() = function
  if game_state == "menu" then
    screen.clear("#000033")
    screen.text(game_config.title, 0, 40, 24, "gold")
    screen.text("SPACE to start, L to load", 0, 5, 14, "white")
    screen.text("arrows move, SPACE shoots, E talks to the Elder", 0, -25, 9, "#8888AA")
    screen.text("H heals, S saves, R risks, P pauses", 0, -40, 9, "#8888AA")
  elsetry game_state == "paused" then
    screen.clear("#111111")
    screen.text("PAUSED", 0, 15, 28, "white")
    screen.text("P to go on", 0, -15, 14, "#AAAAAA")
  elsetry game_state == "gameOver" then
    screen.clear("black")
    screen.text("GAME OVER", 0, 25, 30, "red")
    screen.text("Score " + player.score, 0, -5, 16, "white")
    screen.text("SPACE to try again", 0, -35, 13, "#AAAAAA")
  elsetry game_state == "win" then
    screen.clear("#002200")
    screen.text("VICTORY!", 0, 30, 32, "gold")
    screen.text("The Crystal of Harmony is whole", 0, 0, 14, "white")
    screen.text("Final score " + player.score, 0, -22, 15, "#AAFFAA")
    screen.text("SPACE for the menu", 0, -50, 12, "#AAAAAA")
  else
    draw_game()
  end
end

draw_game() = function
  screen.clear("#141428")
  view x=player.x * 0.4, y=player.y * 0.4 do
    screen.outlineRect(0, 0, 476, 296, "#2A2A50")
    for c in crystals do
      if not c.taken then c.draw() end
    end
    for n in npcs do
      n.draw()
    end
    for e in enemies do
      if e.alive then e.draw() end
    end
    for b in bullets do
      if b.alive then b.draw() end
    end
    player.draw()
    if nearby then screen.text("E to talk", nearby.x, nearby.y - 18, 9, "white") end
  end

  screen.text("Score " + player.score, -130, 88, 12, "gold")
  screen.text("Crystals " + player.crystals + "/" + game_config.crystals_needed, 0, 88, 12, "white")
  screen.text("HP " + player.hp + "  Potions " + player.potions, 130, 88, 12, "white")

  if player.hp < game_config.start_hp * 0.3 then
    screen.text("CRITICAL HP!", 0, 70, 13, "red")
    alter opacity=0.15 do
      screen.fillRect(0, 0, 420, 220, "red")
    end
  elsetry player.hp < game_config.start_hp * 0.6 then
    screen.text("Injured", 0, 70, 12, "orange")
  else
    screen.text("Healthy", 0, 70, 12, "#66DD66")
  end

  if dialogue_timer > 0 then
    alter opacity=0.85 do
      screen.fillRound(0, -55, 300, 26, 0.3, "#111827")
    end
    screen.text(dialogue_text, 0, -55, 11, "gold")
  end

  screen.text("SPACE shoot  E talk  H heal  S save  R risk  P pause", 0, -88, 9, "#555577")
end
```

**Try it.** Make the guards faster with `enemy_speed`, then give the hero a second weapon: a `key("x").hit` that fires three bullets at once, one straight and two to the sides.

## 28. Graduation

### You are a TiberScript 1.1 Developer!

You have learned the full language. Now go forth and create without spaghetti code.

**What you know:**

* Comments `//` and `/* */`
* Variables, functions, `local`, `return`, functions as values
* Booleans, `null`, what counts as false, operators, `%`
* `if`, `elsetry`, `else`
* `for` with `by`, `for ... in`, `while`, `repeat`, `every`, `break`, `continue`
* Strings: `.length`, `.charAt`, `.indexOf`, `.startsWith`, `.split`, `.toUpper` and the rest
* Arrays: `push`, `removeAt`, `contains`, `join`, indexing
* `data` blocks
* `during`, `transit`, `Tiber.state`, `init()`, `update()`, `draw()`
* `key().hit`, `key().held`, `mouse`
* `alter` with opacity, scale, rotation and color, `view`, all drawing tools
* Audio: `sfx`, `tune`, `silence`, `pause`, `resume`, `alter volume= pitch=`
* `mark`, `collide`, `near`, `vanish`, `distance`
* `gravity`, `applyPhysics`, `bounce`
* `archetype` / `class`, `new`, `extends`, `super`
* `attempt`, `recover`, `debug`, `Tiber.lastError`
* `tween`, `shake`, `flash`
* `access` saving/loading
* `emit`, `schedule`
* `Tiber.addCommand`, `Tiber.addScript`, `runScript`

A simple language inspired by microScript, made even simpler. Go build something!
