# TiberScript 1.0 Tutorial!

> **This is the 1.0 Tutorial, checked on 11/24/2026.** Every example runs on today's TiberScript engine in microStudio, and the final test and the whole game after it are played to both of their endings by microStudio's own tests. After this course, the 1.1 Tutorial teaches the rest of 1.1, and the 1.2 Tutorial teaches the newest version. The 1.0 Handbook has everything this course teaches in full.

## 1. Welcome to TiberScript

In this course, we will learn **TiberScript 1.0**.

TiberScript is designed to eliminate "spaghetti code" by using **Scoped Logic** and **Readable Intent**.

## 2. The Workspace

TiberScript is one of microStudio's languages. microStudio's engine reads your TiberScript and turns it into a game.

1. **Make a project** in microStudio, and pick **TiberScript** as its language in the project's settings.
2. **`main`**: Your workspace. Write your game in this file, in the Code tab.
3. **Press Run.** The game plays in the window next to your code.

## 3. Core Syntax: Functions & Variables

TiberScript uses an assignment based syntax. It treats functions as data to keep things clean.

**Standard Code:**

```lua
-- Lua
function jump() ... end

```

**TiberScript:**

```tiberscript
jump() = function
    print("Jump!")
end

speed = 10

```

It reads like a math equation: *"Jump equals a function."*

## 4. Logic Control (Elsetry)

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

## 5. Loops (For)

Use `for` to repeat logic a set number of times. Great for spawning enemies, drawing coins, or anything that repeats.

```tiberscript
for i = 1 to 3
    print("Enemy " + i + " spawned!")
end

```

You can also loop over a list with `for ... in`:

```tiberscript
items = ["sword", "shield", "potion"]

for item in items
    print("You found: " + item)
end

```

> **Tip:** `for` works great inside a `during` block. Use it to update or draw many objects at once.

## 6. Game Flow (States)

This is the most important part of TiberScript. Never use variables like `if state == "menu"` to track where you are.

Use **`during`** blocks.

```tiberscript
during "menu" do
    screen.clear("#111")
    screen.text("Press SPACE")
    if key("space").hit then transit("game") end
end

during "game" do
    screen.clear("#223")
    player.x += 1
    screen.text("Playing!", player.x, 0)
end

```

* **`transit("name")`**: Switches the engine to a new state immediately.

A game made of nothing but `during` blocks runs them every frame and starts in the first one. A bigger game puts them inside an `update()` function, as the final test does.

An `if` on one line still ends with `end`: `if key("space").hit then transit("game") end`.

## 7. Input System: Intent vs Action

TiberScript distinguishes between a **Tap** and a **Hold**.

* **`.hit`**: Returns `true` *once* when pressed (Jumping, Shooting).
* **`.held`**: Returns `true` *continuously* while down (Walking).

```tiberscript
if key("arrowRight").held then player.x += 2 end
if key("space").hit then jump() end

```

## 8. Visual Scopes

In other languages, if you change transparency, you have to remember to change it back. Here, we use **Scopes**.

**`alter ... do`** applies effects only to the code inside the block. It knows `opacity`, `scale`, `rotation` (in degrees) and `color`, which tints a sprite that color.

```tiberscript
// Only this ghost is transparent!
alter opacity=0.5, color="red" do
    screen.sprite("ghost", x, y)
end

// The next drawing is normal again.
player.draw()

```

## 9. The Tag System (Physics)

Do not check for specific objects manually. Use **Tags**.

1. **`this.mark("tag")`**: Label an object in its `init`.
2. **`collide("tag")`**: Checks for collision. It gives back the object it touched, or `null`.

```tiberscript
// Inside the player's update() method
coin = collide("currency")

if coin then
    this.money += 1
    coin.vanish() // Removes the coin
end

```

## 10. Archetypes (OOP)

We call Classes "Archetypes". `init()` runs when you make one with `new`.

```tiberscript
enemy = archetype
    init() = function
        this.hp = 100
        this.mark("hazard")
    end
    die() = function
        print("Enemy Down")
    end
end

grunt = new enemy()

```

## 11. Final Test: The Dungeon of Tiber

Copy the code below into `main` and press Run. Walk with the arrow keys to the gold goal on the right, and stay away from the red enemies.

```tiberscript
config = data
    hp = 100
end

hero = archetype
    init() = function
        this.x=-140
        this.y=0
        this.hp=config.hp
        this.mark("player")
    end
    draw() = function
        screen.sprite("icon",this.x,this.y)
    end
end

goal = archetype
    init() = function
        this.x=140
        this.y=0
        this.mark("win")
    end
    draw() = function
        alter color="gold" do
            screen.sprite("icon",this.x,this.y)
        end
    end
end

enemy = archetype
    init() = function
        this.x=random(120)-60
        this.y=random(160)-80
        this.mark("hazard")
    end
    draw() = function
        alter color="red" do
            screen.sprite("icon",this.x,this.y)
        end
    end
end

init() = function
    player = new hero()
    exit = new goal()

    // Spawn 3 enemies using a for loop
    enemies = []
    for i = 1 to 3
        enemies.push(new enemy())
    end

    transit("play")
end

update() = function
    during "play" do
        screen.clear("#222")
        screen.text("Find the Goal", 0, 85)
        
        if key("arrowRight").held then player.x+=2 end
        if key("arrowLeft").held then player.x-=2 end
        if key("arrowUp").held then player.y+=2 end
        if key("arrowDown").held then player.y-=2 end
        if collide("hazard") then transit("caught") end
        if collide("win") then transit("victory") end
        
        view world do
            player.draw()
            for e in enemies
                e.draw()
            end
            exit.draw()
        end
    end

    during "caught" do
        screen.clear("#300")
        screen.text("CAUGHT!", 0, 10, "red")
        screen.text("Press SPACE to try again", 0, -20)
        if key("space").hit then init() end
    end

    during "victory" do
        screen.clear("gold")
        screen.text("VICTORY!", 0, 0, "#222")
    end
end

```

## 12. The Whole Game: Tiberian Odyssey

The Final Test was a level. This is a whole game, in one file, with every lesson of this course in it. Copy it into `main` and press Run.

The Crystal of Harmony has been shattered. Elder Tim asks you to find five crystals that float around the field and bring them back to him, while four red guards watch the corners. A guard that sees you comes after you, and a guard that touches you hurts.

**Controls:** the arrow keys walk, SPACE strikes a guard in reach, E talks to Elder Tim when you stand by him, H drinks a potion, S saves, R tries something risky, P pauses. On the menu, SPACE starts and L loads what you saved.

Everything you learned is in it:

- **`data`** holds the numbers the game is built on, in one place: change `enemy_sight` or `crystals_needed` and play again.
- **Archetypes** for the hero, the guards, the crystals and the people, and **`extends`** with **`super.init()`** for Elder Tim, who is a character with a quest.
- **`during`** and **`transit`** for the menu, the game, the pause and the two endings. A `return` after a `transit` ends the frame there, so the key that changed the state is not read again by the next one.
- **`mark`**, **`collide`**, **`near`** and **`vanish`**: a crystal is taken by walking into it, a guard is struck when it is near, and both leave the world.
- **`alter`** tints the one `icon` every project has into a hero, guards, crystals and an elder, and **`view`** keeps the hero in the middle of the screen.
- **`elsetry`** picks the health line, **`wave`** makes the crystals float, **`schedule`** has the Elder remind you later, **`emit`** throws sparks, **`attempt`** and **`recover`** catch a risk that goes wrong, and **`access`** saves and loads the game, crystals included.

The names in `game_config` that start with `sfx_`, and `music`, are left empty, so the game plays silently. Give each the name of a sound from your project's Sounds tab, and `music` a song from its Music tab, and it plays them.

```tiberscript
// Tiberian Odyssey: a whole game in TiberScript 1.0

game_config = data
  title = "Tiberian Odyssey"
  start_hp = 100
  speed = 2
  enemy_damage = 10
  enemy_sight = 90
  hurt_ticks = 45
  potions = 2
  potion_heal = 25
  crystals_needed = 5
  // names from your Sounds and Music tabs: left empty, the game is silent
  music = ""
  sfx_hit = ""
  sfx_collect = ""
  sfx_heal = ""
  sfx_save = ""
  sfx_strike = ""
  sfx_win = ""
  sfx_lose = ""
end

hero = archetype
  init(name) = function
    this.name = name
    this.hp = game_config.start_hp
    this.x = 0
    this.y = 0
    this.score = 0
    this.crystals = 0
    this.potions = game_config.potions
    this.hurt_timer = 0
    this.mark("player")
  end

  move() = function
    if key("arrowRight").held then this.x += game_config.speed end
    if key("arrowLeft").held then this.x -= game_config.speed end
    if key("arrowUp").held then this.y += game_config.speed end
    if key("arrowDown").held then this.y -= game_config.speed end
    this.x = clamp(this.x, -220, 220)
    this.y = clamp(this.y, -130, 130)
    if this.hurt_timer > 0 then this.hurt_timer -= 1 end
  end

  hurt(amount) = function
    if this.hurt_timer > 0 then return end
    this.hp -= amount
    this.hurt_timer = game_config.hurt_ticks
    sfx(game_config.sfx_hit)
    emit("blood", this.x, this.y, 6)
    if this.hp <= 0 then lose() end
  end

  heal() = function
    if this.potions == 0 then
      say("No potions left")
    elsetry this.hp >= game_config.start_hp then
      say("Already at full health")
    else
      this.potions -= 1
      this.hp = clamp(this.hp + game_config.potion_heal, 0, game_config.start_hp)
      sfx(game_config.sfx_heal)
      emit("magic", this.x, this.y, 10)
      say("A potion: " + this.hp + " HP")
    end
  end

  strike() = function
    local foe = near("enemy", 28)
    if foe then
      foe.defeat()
      this.score += 50
    else
      say("Nothing in reach")
    end
  end

  save() = function
    access "odyssey_save" as file do
      file.hp = this.hp
      file.x = this.x
      file.y = this.y
      file.score = this.score
      file.potions = this.potions
      file.found = found_list()
    end
    sfx(game_config.sfx_save)
    say("Saved")
  end

  load() = function
    if exists("odyssey_save") then
      access "odyssey_save" as file do
        this.hp = file.hp
        this.x = file.x
        this.y = file.y
        this.score = file.score
        this.potions = file.potions
        for i in file.found
          crystals[i].take(this, false)
        end
      end
      say("Welcome back")
    else
      say("No save yet")
    end
  end

  draw() = function
    if this.hurt_timer > 0 then
      alter color="red", scale=1.2 do
        screen.sprite("icon", this.x, this.y)
      end
    else
      screen.sprite("icon", this.x, this.y)
    end
  end
end

enemy = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.alive = true
    this.mark("enemy")
  end

  chase(target) = function
    if not this.alive then return end
    local dx = target.x - this.x
    local dy = target.y - this.y
    if dx * dx + dy * dy < game_config.enemy_sight * game_config.enemy_sight then
      if dx > 1 then this.x += 1 end
      if dx < -1 then this.x -= 1 end
      if dy > 1 then this.y += 1 end
      if dy < -1 then this.y -= 1 end
      if collide("player") then target.hurt(game_config.enemy_damage) end
    end
  end

  defeat() = function
    this.alive = false
    sfx(game_config.sfx_strike)
    emit("spark", this.x, this.y, 16)
    this.vanish()
  end

  draw() = function
    if not this.alive then return end
    alter color="red" do
      screen.sprite("icon", this.x, this.y)
    end
  end
end

crystal = archetype
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
      collector.score += 100
      sfx(game_config.sfx_collect)
      emit("coin", this.x, this.y, 8)
    end
    this.vanish()
  end

  draw() = function
    if this.taken then return end
    alter color="cyan", scale=0.7 do
      screen.sprite("icon", this.x, this.y + wave(ticks + this.index * 20, 4))
    end
  end
end

character = archetype
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
    alter color="#AAAAFF" do
      screen.sprite("icon", this.x, this.y)
    end
    screen.text(this.name, this.x, this.y + 14, "#AAAAFF")
  end
end

elder = archetype extends character
  init() = function
    super.init("Elder Tim", 0, 60)
    this.asked = false
  end

  talk() = function
    if not this.asked then
      this.asked = true
      say("Elder Tim: Find " + game_config.crystals_needed + " crystals for me!")
      schedule(600) = function
        if npcs[0].asked and not won then say("Elder Tim: Keep searching, the crystals glow cyan") end
      end
    elsetry player.crystals >= game_config.crystals_needed then
      say("Elder Tim: The Crystal of Harmony is whole!")
      win()
    else
      say("Elder Tim: " + (game_config.crystals_needed - player.crystals) + " more to find")
    end
  end
end

player = null
enemies = []
crystals = []
npcs = []
message = ""
message_timer = 0
ticks = 0
won = false

say(text) = function
  message = text
  message_timer = 150
end

found_list() = function
  local found = []
  for c in crystals
    if c.taken then found.push(c.index) end
  end
  return found
end

clear_world() = function
  if player then player.vanish() end
  for e in enemies
    if e.alive then e.vanish() end
  end
  for c in crystals
    if not c.taken then c.vanish() end
  end
  for n in npcs
    n.vanish()
  end
end

new_world() = function
  clear_world()
  player = new hero("Hero")
  enemies = []
  enemies.push(new enemy(-190, 110))
  enemies.push(new enemy(190, 110))
  enemies.push(new enemy(-190, -110))
  enemies.push(new enemy(190, -110))
  crystals = []
  crystals.push(new crystal(0, -150, -70))
  crystals.push(new crystal(1, 150, -70))
  crystals.push(new crystal(2, -150, 80))
  crystals.push(new crystal(3, 150, 80))
  crystals.push(new crystal(4, 0, -100))
  npcs = []
  npcs.push(new elder())
  message = ""
  message_timer = 0
  won = false
end

win() = function
  won = true
  silence("all")
  sfx(game_config.sfx_win)
  transit("win")
end

lose() = function
  silence("all")
  sfx(game_config.sfx_lose)
  transit("gameOver")
end

init() = function
  new_world()
  transit("menu")
end

update() = function
  ticks += 1

  during "menu" do
    screen.clear("#000033")
    screen.text(game_config.title, 0, 40, "gold")
    screen.text("SPACE to start, L to load", 0, 0)
    screen.text("arrows move, SPACE strikes, E talks", 0, -30, "#8888AA")
    screen.text("H heals, S saves, R risks, P pauses", 0, -45, "#8888AA")
    if key("space").hit then
      tune(game_config.music)
      transit("game")
      return
    end
    if key("l").hit then
      player.load()
      tune(game_config.music)
      transit("game")
      return
    end
  end

  during "game" do
    screen.clear("#141E28")
    player.move()

    for e in enemies
      e.chase(player)
    end

    local gem = collide("crystal")
    if gem then gem.take(player, true) end

    local npc = near("npc", 40)
    if npc and key("e").hit then npc.talk() end

    if key("space").hit then player.strike() end
    if key("h").hit then player.heal() end
    if key("s").hit then player.save() end
    if key("p").hit then
      transit("paused")
      return
    end

    if key("r").hit then
      attempt
        // one time in three the chest is opened, and there is no chest
        if random(3) == 1 then
          chest.lock.open()
        end
        player.score += 25
        say("A risk that paid: +25")
      recover
        say("The risk went wrong")
        player.hurt(15)
      end
    end

    view x=player.x, y=player.y do
      for c in crystals
        c.draw()
      end
      for n in npcs
        n.draw()
      end
      for e in enemies
        e.draw()
      end
      player.draw()
      if npc then
        screen.text("E to talk", npc.x, npc.y - 16, "white")
      end
    end

    if player.hp > game_config.start_hp * 0.6 then
      screen.text("Healthy", 0, 88, "#66DD66")
    elsetry player.hp > game_config.start_hp * 0.3 then
      screen.text("Injured", 0, 88, "orange")
    else
      screen.text("Critical!", 0, 88, "red")
      alter opacity=0.15 do
        screen.fillRect(0, 0, 400, 220, "red")
      end
    end

    screen.text("HP " + player.hp, -130, 88)
    screen.text("Crystals " + player.crystals + "/" + game_config.crystals_needed, 120, 88, "cyan")
    screen.text("Score " + player.score + "  Potions " + player.potions, 0, -88, "#AAAAAA")

    if message_timer > 0 then
      message_timer -= 1
      screen.text(message, 0, -70, "gold")
    end
  end

  during "paused" do
    screen.clear("#111111")
    screen.text("PAUSED", 0, 10)
    screen.text("P to go on", 0, -15, "#AAAAAA")
    if key("p").hit then
      transit("game")
      return
    end
  end

  during "gameOver" do
    screen.clear("black")
    screen.text("GAME OVER", 0, 20, "red")
    screen.text("Score " + player.score, 0, 0)
    screen.text("SPACE to try again", 0, -25, "#AAAAAA")
    if key("space").hit then
      new_world()
      transit("menu")
    end
  end

  during "win" do
    screen.clear("#002200")
    screen.text("VICTORY!", 0, 30, "gold")
    screen.text("The Crystal of Harmony is whole", 0, 5)
    screen.text("Final score " + player.score, 0, -15, "#AAFFAA")
    screen.text("SPACE for the menu", 0, -40, "#AAAAAA")
    if key("space").hit then
      new_world()
      transit("menu")
    end
  end
end
```

**Try it.** Give the guards a wider `enemy_sight` and see how much harder the corners get. Then add a sixth crystal: one more line in `new_world()` and `crystals_needed = 6`.

## 13. Graduation

### You are a TiberScript Developer!

You have learned the syntax. Now go forth and create without spaghetti code.

**Quick Reference:**

* `during "state"`
* `transit("state")`
* `alter ... do`
* `collide("tag")`
* `for i = 1 to n`

And that's it. The 1.1 Tutorial is next. A simple language inspired by another simple language, microScript, made even simpler.
