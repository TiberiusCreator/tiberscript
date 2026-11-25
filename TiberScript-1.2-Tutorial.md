# TiberScript 1.2 Tutorial!

> **This is the 1.2 Tutorial, checked on 11/24/2026.** Every code block in it, the final test's Boss Arena included, is run on the 1.2 engine by microStudio's own tests, and the whole game after it is played to both of its endings. It follows the 1.1 Tutorial, and the 1.2 Handbook has everything in full. Each lesson names the part of the Handbook it comes from where there is more to read.

## 1. Welcome Back

This course continues straight from TiberScript 1.1 Tutorial. If you have not graduated that one yet, go do that first. Everything below assumes you already know `archetype`, `during`, `key().hit`, and the rest of 1.1.

1.2 is the game script upgrade. Every 1.1 script still runs as it did, with one small exception you will meet in Lesson 3. This course is about what got added on top, and it ends with the biggest thing of all, in six parts: a game that can rewind itself and play online in one word, a crowd of a hundred thousand bullets that never stutters, a compiler that lets you change the game while it runs, takes whole blocks of JavaScript right inside TiberScript, and makes the busiest code several times faster with one word, shaders: TiberScript the graphics chip runs for every pixel, for skies, water and black holes in a few lines, pages written in the game's own file, HTML whose values keep themselves up to date, and the biggest of them all, changing the past: a change made two seconds ago, and the present it would have made. Then light: torches, flashlights, sunlight and real shadows in a line each.

## 2. Modules: import and export

One file cannot hold a whole game forever. 1.2 lets you split your project into real files.

```tiberscript
// enemies.tbs
export slime = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.mark("enemy")
  end
end
```

```tiberscript
// main.tbs
import "enemies"

init() = function
  boss = new enemies.slime(0, 0)
end
```

Only things marked `export` are visible to files that `import` you. Everything else stays private to its own file. Imports always go at the top of a file.

An exported value is shared, not copied. If a file says `export gravity = 9.8`, then `rules.gravity = 20` from another file changes it for both, which is how a library lets you set its options. `export const LIMIT = 3` can be read from anywhere and never changed.

A module belongs to its project. To use one in every project you make, make it a Tiber library: put `Tiber.create("tools")` at the top of the file and run the game once in the editor. From then on, every one of your TiberScript projects reaches what that file exports as `Tiber.tools`:

```tiberscript
init() = function
  if Tiber.tools != null then
    best = Tiber.tools.double(4)
  end
end
```

The library panel lists your Tiber libraries, and `Tiber.libraries` lists them in code. Section 3 of the handbook has the rest.

## 3. Small Things That Help Every Day

Strings can hold values directly:

```tiberscript
screen.text("Score: $(score)  Lives: $(lives)", 0, 90, 12, "white")
```

Anything that counts frames also takes seconds:

```tiberscript
sequence do
  wait(seconds(1.5))
end
every(seconds(2)) do
  spawnSlime()
end
```

And a `for` loop's counter now belongs to its loop. In 1.1 this was a trap:

```tiberscript
spawnRow() = function
  for i = 1 to 3 do
    spawnSlime(i * 20)
  end
end

for i = 1 to 5 do
  spawnRow()   // in 1.1 this changed our i. In 1.2 it cannot.
end
```

That is the one change from 1.1: if an old script reads a loop counter after its loop ends, copy the value into your own variable inside the loop.

An `if` with more than two ways to go reads `elsetry` for each one after the first, and `elseif` is the same word written another way. Either one is fine, the way `archetype` and `class` are:

```tiberscript
medal(time) = function
  if time < 30 then
    return "gold"
  elseif time < 45 then // or elsetry: either one is fine
    return "silver"
  else
    return "bronze"
  end
end
```

Or leave the old script exactly as it is, and tell 1.2 which version it was written for:

```tiberscript
tiberscript 1.1
  lastLap(n) = function
    for lap = 1 to n do
    end
    return lap
  end
end

init() = function
  laps = lastLap(3)
  now = Tiber.version
end
```

Everything between `tiberscript 1.1` and `end` is read by the 1.1 rules, so `laps` is `3`, the way the function always worked, while the code around it is 1.2 and `now` is `"1.2"`. `tiberscript 1.0 ... end` does the same for 1.0. For a whole old file, one line at its top is enough: `Tiber.version = "1.1"`. That is how a game you wrote long ago moves into a new project without a single line changed.

## 4. The Standard Library: string and array

1.1 already has `.trim()`, `.split()`, `.toUpper()`, `.contains()`, `.join()` and the rest on strings and lists. 1.2 adds the helpers that need more than one thing to work with:

```tiberscript
msg = string.format("Score: $0 of $1", score, maxScore)
padded = "7".padLeft(3, "0") // "007"
joined = string.join(parts, " | ") // the same as parts.join(" | ")
```

```tiberscript
isAlive(e) = function
  return e.hp > 0
end

survivors = array.filter(enemies, isAlive)
found = array.find(inventory, isKey)
```

`array.filter` and `array.find` take a plain function, the same kind you already write for `Tiber.addCommand()`. You can also write the function right where you use it:

```tiberscript
survivors = array.filter(enemies, function(e)
  return e.hp > 0
end)
```

That is an inline function: `function`, its parameters, its body, `end`, with no name.

## 5. vector and math

```tiberscript
v = vector(3, 4)
print(v.length()) // 5

dir = v.normalize()
angle = math.atan2(dy, dx)
```

`vector` gives you a real point with its own methods, instead of tracking x and y as two separate numbers everywhere.

## 6. table

`data` blocks have a fixed shape. `table` is for when you do not know the shape ahead of time.

```tiberscript
for key in table.keys(config)
  print(key + " = " + config[key])
end

value = table.get(config, "difficulty", "normal")
```

`table.get` takes a default, so a missing key never crashes your game.

## 7. Traits: Sharing Behavior With uses

`extends` only lets an archetype have one parent. A player and a crate are nothing alike, but both might need to take damage. That is what a trait is for.

```tiberscript
damageable = trait
  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then this.die() end
  end
end

crate = archetype uses damageable
  init() = function
    this.hp = 20
  end
  die() = function
    this.vanish()
  end
end
```

Chain as many as you need with `and`:

```tiberscript
hero = archetype uses damageable and flammable and swimmable
  init() = function
    this.hp = 100
  end
end
```

## 8. Tunable Variables: export

Tuning numbers by hand in code is slow. Mark a value `export` and it becomes editable from a panel in the editor, no code changes needed.

```tiberscript
enemy = archetype
  export moveSpeed = 2
  export attackDamage = 10

  init() = function
    this.hp = 50
  end
end
```

Run the game and press the sliders button in the run window's toolbar: every `export` value of your game is there, with a slider. Drag one and the running game changes at once. When a value feels right, press write to code and the new number is written into the `export` line of your source.

## 9. Enums: Naming Your States

A typo in `transit("mnue")` fails silently and your screen just goes blank. `enum` catches that.

```tiberscript
states = enum
  menu
  game
  paused
  gameOver
end

transit(states.menu)

during states.menu do
  if key("space").hit then transit(states.game) end
end
```

## 10. Entering and Leaving a State

`during` runs every frame. For things that should happen once, when a state starts or ends:

```tiberscript
entering states.game do
  tune("level_theme")
end

entering states.gameOver do
  save.set("highScore", max(score, save.get("highScore", 0)))
  save.write()
end
```

No more "did I already save?" flags.

## 11. Sequences: wait and Cutscenes

Cutscenes used to mean a manual counter and a pile of `if timer == 60` checks. `sequence` lets you write it top to bottom, like a script.

```tiberscript
entering states.bossIntro do
  sequence do
    caption = "A boss appears!"
    wait(60)
    shake(6, 20)
    wait(30)
    caption = ""
    transit(states.boss)
  end
end

update() = function
  during states.bossIntro do
    screen.text(caption, 0, 0, 20, "white")
  end
end
```

`wait(N)` pauses just that sequence. The rest of your game keeps running. The sequence changes `caption`, and `update()` draws it every frame: anything drawn only once is gone the next frame.

`wait()` also takes a signal instead of a number, pausing until that signal fires instead of guessing a frame count:

```tiberscript
sequence do
  trigger("spawnBoss", null)
  wait(boss.introFinished)
  caption = "Fight!"
end
```

## 12. Quick Events: on and trigger

```tiberscript
on("enemyDied") do (info)
  score += info.points
end

enemy = archetype
  die() = function
    trigger("enemyDied", data(points = 100))
    this.vanish()
  end
end
```

Any number of `on()` listeners can hear the same `trigger()`. `enemy` never needs to know who is listening.

## 13. Declared Signals

Global events are quick, but on a big project it helps to see what one object can announce just by reading its own definition.

```tiberscript
hero = archetype
  signal died()

  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then this.died.trigger() end
  end
end

init() = function
  player = new hero()
  player.died.connect(onPlayerDied)
end

onPlayerDied() = function
  transit(states.gameOver)
end
```

Name the archetype and the variable differently (`hero` and `player`): `player = new player()` would replace the archetype with the object, and there would be nothing left to make a second one from.

## 14. Controllers

Plug in an Xbox, PlayStation or Switch controller, or pair one over Bluetooth, and press any button on it: a browser only shows a controller to a game after one of its buttons is pressed.

```tiberscript
if gamepad(0).hit("south") then jump() end
if gamepad(0).axis("leftX") > 0.5 then player.x += 3 end
```

`gamepad(0)` is player 1's controller, `gamepad(1)` player 2's. Same `.hit` and `.held` pattern keyboard already uses.

Buttons are named by where they are, because every brand prints its own letters on them: `"south"` is the bottom face button (A on Xbox, Cross on PlayStation, B on Switch), `"east"` the right one, `"west"` and `"north"` the others. `"lb"`, `"rb"`, `"lt"`, `"rt"`, `"start"`, `"select"`, and `"up"`, `"down"`, `"left"`, `"right"` (the D pad or the left stick) are the rest. Each brand's own names work too: `"cross"`, `"zr"`, `"plus"`. And `"confirm"` is the button that means yes on that controller: the bottom one on Xbox and PlayStation, the right one on a Switch.

Show the player the button they actually have:

```tiberscript
draw() = function
  screen.clear()
  local pad = controllers.first
  if pad then
    pad.drawButton("south", -30, 0, 12)
    screen.text("to jump", 10, 0, 8, "#FFFFFF")
  end
end
```

`drawButton` draws a green A for an Xbox controller, a blue cross for a PlayStation one and a B for a Switch Pro Controller, lit while it is held. `pad.label("south")` is the same as text, and `pad.draw(x, y, width)` draws the whole controller.

Make it rumble when the hero is hurt with `pad.rumble(0.6, 0.2)`: strength from 0 to 1, and seconds. A controller the browser does not lay out the standard way has `pad.setUp` false, and `controllers.setup()` opens a screen that asks for each of its buttons once and remembers them. If a controller is unplugged in the middle of a fight, `on("controllerDisconnected")` hears it, and the player keeps their number when they plug it back in.

A light press is enough: a button counts from a fifth of the way down, and so does a stick for `"left"` and the rest. The engine also looks at every controller between two updates, so the quickest tap is never missed. A left and a right Joy Con the browser lists apart become one controller (`pad.layout` is then `"joycon pair"`).

One thing helps when a controller misbehaves, in any TiberScript game: print `controllers.report()`: one line for every controller the browser sees, with what is pressed right now, and one for how the engine reads it. A controller that is listed but silent is the browser's or the computer's doing, not the game's.

A button can say more than pressed: how long it has been held, a tap or a long press, a hold that repeats for a menu, and how far down it has to go before it counts. That is Lesson 50.

**Controllers read directly.** A browser reads a controller and hands the game what it understood, at its own pace, and some browsers are slow at it: a controller then answers late in every game. On the computer microStudio runs on, the engine does better. microStudio's controller bridge reads every controller itself (Nintendo's Pro Controller and Joy Cons, the Switch 2's Joy Con 2 and Pro Controller, Sony's DualShock 4 and DualSense, Microsoft's Xbox controllers over Bluetooth, and any other gamepad) and passes every report to your game the moment it arrives. Nothing is picked and nothing is set: the controller is on, and it plays, as quickly as it does on its own console. It also brings what a browser leaves out: a Pro Controller that a browser cannot read at all, each stick's own calibration, the motion sensor, the battery, rumble, and the light that shows which player it is. In Chrome and Edge there is a second way for a Nintendo controller, for a game played from somewhere else: the game calls `controllers.connect()`, the player picks the controller once in the browser's list, and it is opened by itself from then on.

Your code does not change, and four names are new. `pad.direct` says who reads the controller (`"bridge"`, `"webhid"`, or `""` for the browser). `pad.pace` is how many thousandths of a second pass between two readings: around 15 is a controller read at its own pace, and a number in the hundreds is a browser being slow. `pad.motion` is the motion sensor (`pad.motion.ax` is how far it leans to the side, in g), and `pad.battery` is `"full"`, `"low"` and so on.

```tiberscript
update() = function
  local p = gamepad(0)
  if p.motion != null then
    hero.x += p.motion.ax * 3
  end
end
```

## 15. Action Mapping: bind and action

Hardcoding `key("arrowRight")` everywhere means no rebindable controls. `bind` and `action` fix that.

```tiberscript
bind("moveRight", "arrowRight")
bind("moveRight", "d")
bind("jump", "space")
bind("jump", controllers.button("south"))
bind("jump", touch.button("jump", 110, -70, 18))

if action("moveRight").held then player.x += 3 end
if action("jump").hit then player.vy = 8 end
```

`controllers.button("south")` is that button on whichever controller presses it. `touch.button` puts a button on the screen for phones, and only there.

## 16. Sprite Animation

```tiberscript
player.animate("walk", ["hero_walk1", "hero_walk2", "hero_walk3"], 8)
player.animate("idle", ["hero_idle1", "hero_idle2"], 20)

if action("moveRight").held then
  player.playAnimation("walk")
else
  player.playAnimation("idle")
end
```

You can also pick the frame yourself: `screen.sprite("hero_run.2", x, y, 16)` draws frame 2 of the sprite `hero_run` (frames count from 0), and `screen.sprite("hero_attack", x, y, -24, 16)` draws a sprite in a box of its own, mirrored by the negative width. A game can pick its run frame from how far the hero has walked, and play a soft step on the two frames where a foot lands.

## 17. Camera: follow, bounds, layer

```tiberscript
camera.follow("player", 0.1)
camera.bounds(-500, -300, 500, 300)
camera.layer("background", 0.3)
```

`follow` smoothly tracks a target. `bounds` stops the camera at the level edge. `layer` gives you parallax.

## 18. Draw Layers

```tiberscript
screen.layer(0)
background.draw()

screen.layer(1)
player.draw()

screen.layer(2)
ui.healthbar(player.hp, 100, -100, 90, 200, 16)
```

Higher layers always draw on top, no matter what order the calls happen in.

## 19. Tween Easing

```tiberscript
tween("player", "scale", 2, 20, ease = "bounce")
tween("door", "y", 100, 40, ease = "easeOut")
```

Options are `linear`, `easeIn`, `easeOut`, `easeInOut`, `bounce`, `elastic`, `back`, `anticipate`, `smooth`, `sine` and `expo`.

## 20. audio Channels

```tiberscript
audio.setVolume("music", 0.5)
audio.setVolume("sfx", 0.8)

if audio.has("roar") then sfx("roar") end
```

Every `sfx()` and `tune()` call respects its channel automatically. `audio.has` tells you whether the project has that sound at all. Sounds you make in microStudio's Sound Studio and songs from its Music Studio are ordinary project sounds, so `sfx("coin")` just works.

## 21. UI Primitives

```tiberscript
ui.button("Start", 0, 0, 120, 40) = function
  transit(states.game)
end

ui.healthbar(player.hp, 100, -100, 90, 200, 16)
ui.textbox("Quest text that wraps automatically.", -100, 0, 200)
```

## 22. Dialogue and choice

```tiberscript
dialogue "Welcome, traveler." speaker="Elder" do
  wait(60)
  choice "Accept" do
    transit(states.quest)
  end
  choice "Not now" do
    transit(states.game)
  end
end
```

`Tiber.talking` is true while a box is on screen, handy for holding the hero still while the player reads: `if not Tiber.talking then applyPhysics("player") end`.

## 23. Bulk Objects

```tiberscript
coins = array.ofObjects("currency")
closest = array.sort(coins, byDistance)
```

Collects every registered object with a given tag into a real array.

## 24. Fixed Timestep: fixedUpdate

`update()` runs once per rendered frame, and frame rate varies by device. Physics run at an inconsistent rate is a real bug. `fixedUpdate()` runs at a steady rate no matter what.

```tiberscript
fixedUpdate() = function
  applyPhysics("player")
end

update() = function
  if action("jump").hit then player.vy = 8 end
end
```

Remember this split. It is the key that unlocks Lessons 33 to 35.

## 25. Mass and Friction

```tiberscript
boulder.mass = 5
boulder.friction = 0.2
applyPhysics("boulder")
```

Heavier objects respond to force more slowly. Friction slows things down over time.

## 26. Real Collision Shapes and Layers

```tiberscript
this.collisionShape("circle", 16)

bullet.collisionLayer("playerBullet")
bullet.collidesWith("enemy")

local hit = collide("enemy")
if hit then resolve(this, hit) end
```

`collidesWith` means a check only ever runs against layers you actually care about. `resolve()` pushes two objects apart instead of just reporting overlap.

## 27. Testing: test and assert

```tiberscript
test "heal never exceeds max hp" do
  hp = 90
  heal(25)
  assert(hp == 100)
end
```

`test` blocks do not run during normal play. They run on demand, so you can trust your own code as your game grows.

Some mistakes are found before any test runs. When your game starts in the editor, all of its files are read together, and the console says, with the file and the line, a name you misspelled and the one you meant, a function you call that nobody made, a function given more values than it takes, a built in you misspelled (`screen.fillrect`), and a sprite or sound your project does not have:

```text
In main, line 12: 'scroe' is read but never set anywhere in the game: did you mean score?
In main, line 19: sfx(): the project has no sound 'jmp': did you mean 'jump'?
```

The game still runs, as it would have. Section 12 of the handbook has every kind.

## 28. The Tiber Namespace, Expanded

```tiberscript
Tiber.extendArchetype("enemy", "onHit", function(damage)
  screen.text("-$(damage)", this.x, this.y + 20, 12, "red")
end)

if Tiber.hasFeature("timeline") then
  time.keep(seconds(10))
end

print(Tiber.version)
```

`Tiber.extendArchetype` customizes an archetype from outside the file it was defined in. `Tiber.hasFeature` lets a shared script check what the engine can do, alongside `Tiber.version`, which 1.1 already had.

```tiberscript
Tiber.showDebugOverlay(true)
```

Draws a live panel showing every `trigger()` as it fires, the tags on whatever is under your mouse, and where each frame's time went. Good for chasing down a collision that is not working the way you expect, or a frame that is slow.

Two more that make a game smooth on every computer:

```tiberscript
init() = function
  Tiber.frameRate = "auto"
  Tiber.renderScale = "auto"
end

draw() = function
  screen.clear()
end
```

Your game always updates 60 times a second. `Tiber.frameRate` is how many pictures it draws: 60 (the default), 120 (on a 120 Hz screen, `draw()` also runs between updates, with everything that moves shown part of the way, so motion is twice as smooth and the game plays at the same speed), 240 (the same, for a screen that refreshes 240 times a second), or `"auto"` (as many as your screen shows, up to 240, while there is room for it). `Tiber.renderScale` draws fewer pixels on a computer whose graphics cannot fill a big window in time: 0.75, 0.5, or `"auto"`. Section 21 of the handbook lists the computers each setting runs on, down to a 2016 laptop and a Raspberry Pi.

To try your game at another frame rate without changing any code, open the performance monitor: add the PerfMonitor library to the game (Import Libraries, under Built In Libraries), give it a key in your `init()` with `PerfMonitor.useKey("m")`, then press M in the running game. Its **pictures** switch goes to GAME, 60, 120, 240 and AUTO (as many pictures as your monitor shows), and the running game changes at once. Its **updates** switch does the same for a game written to run at any number of updates a second (`Tiber.tickRate = "auto"`).

And one for how sharp the picture is, the way a game's settings menu says it:

```tiberscript
init() = function
  Tiber.resolution = "1080p"
end

draw() = function
  screen.clear()
  screen.text("$(Tiber.frameWidth) by $(Tiber.frameHeight)", 0, 0, 10, "#FFFFFF")
end
```

`Tiber.resolution` is how many lines high the game's picture is, whatever the size of the window: `"720p"` for an old laptop, `"1080p"`, `"1440p"`, `"4k"`, or `"native"` for the window's own pixels, which is where a game starts. Nothing in your game moves or changes size, because the screen is always 200 units high. Only how finely it is drawn changes.

## 29. Structured Save Data

```tiberscript
save.set("highScore", 5000)
best = save.get("highScore", 0)
save.write()

slots = save.list()
save.delete("slot3")
```

To save a whole archetype instance at once instead of one field at a time:

```tiberscript
save.setObject("player", player)
player = save.getObject("player", new hero())
```

And to save the whole game at once, every object, list, swarm, the state and the random numbers, in one line:

```tiberscript
save.game("slot1")
save.loadGame("slot1")
```

`save.loadGame` puts everything back into the objects the game already has, so the game plays on from there exactly as it did when it was saved. `save.hasGame(slot)` and `save.gameInfo(slot)` tell whether and when a game was saved.

## 30. Networking and Multiplayer

```tiberscript
network.on("playerJoined") do (who)
  register("player_" + who.id, 0, 0)
end

network.send("position", data(x = player.x, y = player.y))
```

Messages from other players come in through `network.on`, kept separate from your own `on` events.

To try it, turn on the networking option in your project's settings. The run window gets a Server bar: press **Start** on it, then open the game in a second window. A game with no `serverUpdate()` of its own gets a server built in that passes every message to every other player, so there is no server code to write. `network.id` is your own player's id, `network.players` the ids of everyone connected, and `network.connected` says whether you are online at all.

Sending positions by hand is how most games start. Lesson 33 shows the better way for action games, rollback, and `online`, the one word that adds a lobby, a link and a QR code to it.

## 31. Cancellable Timers and More Randomness

```tiberscript
respawnTimer = schedule(180) = function
  respawnPlayer()
end

cancelSchedule(respawnTimer)
```

```tiberscript
seedRandom(1234)
f = randomFloat(0, 1)
coin = randomBool()
pick = randomChoice(["sword", "shield", "potion"])
dir = randomSign()
```

`random()` itself never changed. Everything above is new on top of it.

## 32. Tile Maps, Noise and Pathfinding

A map from the Maps tab becomes solid ground in three lines:

```tiberscript
level = tilemap("level1", x = -200, y = -120)
level.solid("ground", "wall")
level.oneWay("ledge")

fixedUpdate() = function
  applyPhysics("player")
  level.collide(player)
  if player.onFloor and action("jump").hit then player.vy = 8 end
end
```

`noise()` is randomness that changes smoothly, for hills and flickering torches:

```tiberscript
h = noise(x * 0.02) * 40
```

And `path.find()` is the chase every guard needs:

```tiberscript
route = path.find(level, from = vector(guard.x, guard.y), to = vector(player.x, player.y))
if route then guard.moveToward(route[1], 1.5) end
```

### Building a World in the Map Editor

Paint the level in the Maps tab and let the game read it. A sprite bigger than one tile is a tileset: each of its squares is a tile, kept in the map as `"stone:3,1"`, and `tileAt()` gives just `"stone"`, so one `solid("stone")` covers the whole tileset.

Make a little tileset of markers too, one square for each thing a room holds: the start, a foe, a bell. Paint them where those things go, and the game finds them:

```tiberscript
level = tilemap("level1", x = -200, y = -120)
level.solid("ground")
for spot in level.find("mark:2,0") do
  print("a foe at " + spot.x + ", " + spot.y)
  level.set(spot.x, spot.y, null)
end
```

`find()` lists every cell with that tile, each with its `x` and `y` in the world, and `set(x, y, null)` rubs the marker out so it is never drawn. Change the painting and the room changes, with no numbers typed at all.

A whole world is many such maps, each a room at its own place on one grid. When the hero walks out of a room, load the room he walked into at the place he is standing, set `camera.bounds()` to it, and the walk goes on. The list of rooms, their names and where they are, can live in a JSON file in the Assets tab, read with `asset_manager.loadJSON("world")`: wait until its `ready` is 1, then its `data` is the list. A table of foes as a CSV file, or the words carved on a wall as a text file, are read the same way with `loadCSV()` and `loadText()`, and `string.toNumber()` turns a cell of the table into a number.

**Try it.** Paint three markers into a map, and make `find()` count them.

## 33. THE BIGGEST UPDATE OF ALL, Part One: Time

Here is the lesson everything else was leading to. Because your game changes the world in `fixedUpdate()`, reads input through actions, and uses seeded randomness, TiberScript can keep its past and see its future.

Turn it on:

```tiberscript
init() = function
  time.keep(seconds(10))
  bind("rewind", "r")
end
```

**Rewind.** Hold R and the whole game runs backwards:

```tiberscript
fixedUpdate() = function
  if action("rewind").held then
    time.rewind(2)
  else
    applyPhysics("player")
  end
end
```

`time.keep(seconds(10))` keeps the last ten seconds. Leave the number out and it keeps the whole game, from the moment it is called, so R can take the player all the way back to the start:

```tiberscript
init() = function
  time.keep()
  bind("rewind", "r")
end
```

Letting go of R carries on from exactly that moment, camera included, so the view never slides. It does not store a copy of every tick to do that. It keeps a picture of the world every so often and the buttons pressed in between, and plays forward from the nearest picture to reach the exact tick. `time.rewindTo(tick)` jumps straight to one tick, and `time.oldest` is the oldest one it can reach. The memory it may use is 256 MB unless you say `time.keep(memory = 64)`; when that fills up, the oldest pictures thin out first.

You decide where time is kept, too. To keep it only during play, so a rewind never goes back into the title screen:

```tiberscript
init() = function
  time.keep(during = states.game)
end
```

Name a moment with `time.mark("checkpoint")` and go back to it with `time.rewindTo("checkpoint")`. `time.forget()` lets go of everything kept so far, `time.stop()` stops keeping time, and `time.atOldest` is true when a rewind can go no further.

**Forecast.** See where the ball will be in a second, with the game's real physics:

```tiberscript
update() = function
  forecast 60 each (future) do
    screen.fillCircle(future.ball.x, future.ball.y, 1.5, "white")
  end
end
```

**What if.** Try something, look at the result, and throw it away:

```tiberscript
whatif do
  ball.vx = 6
  forecast 60 each (future) do
    screen.fillCircle(future.ball.x, future.ball.y, 1.5, "yellow")
  end
end
```

**Replay.** Show the last three seconds again, slowly:

```tiberscript
on("bossDefeated") do (info)
  time.replay(from = time.now - seconds(3), speed = 0.25)
end
```

**Timeless.** Things that must not rewind:

```tiberscript
timeless rewindsUsed = 0
```

**Rollback.** Online play, with no netcode written:

```tiberscript
network.rollback(players = 2, delay = 2)
```

Each player's input is then read with `action(name, player)`, players counted from 0: `action("right", 0)` moves the first hero and `action("right", 1)` the second, in `fixedUpdate()`, on every machine. `action(name)` alone is your own player, `network.me`.

**Online, in one word.** Rollback is how a game plays online. `online` is everything around it:

```tiberscript
init() = function
  bind("right", "arrowRight")
  online
  heroes = [register("blue", -40, 0), register("orange", 40, 0)]
end
```

Put it in `init()` in place of `network.rollback()` and press Run. The run window starts your project's server by itself, and your game waits in a lobby drawn over it, with the room's code, a link and a QR code of the link. Open the link in a second window, or point a phone at the code, and the match starts with rollback underneath. A third player who opens the link watches. A player who leaves can be replaced by the next one who comes, who is handed the game as it is. `online.me` is your seat (or -1 when you watch), `online.status` says what is happening, and `online 4` makes room for four.

**Slowly, and forward again.** A rewind does not have to be fast. `time.rewind(0.5)` goes back at half speed and `time.rewind(0.25)` at a quarter. And after going back, `time.forward()` goes forward again over the same ticks, as long as you have not played a new one: `time.ahead` says how many are left.

```tiberscript
fixedUpdate() = function
  if action("back").held then
    time.rewind(0.5)
  elsetry action("forth").held and time.ahead > 0 then
    time.forward(0.5)
  else
    hero.x += 2
  end
end
```

**Slow motion.** `time.speed` is how fast the whole game goes: 1 as it starts, 0.25 for a quarter, 2 for double. The game plays fewer ticks, and `draw()` shows the world part of the way between two of them, so it is smooth. Most of the time you want it for a moment only, and two functions end by themselves:

```tiberscript
update() = function
  if action("focus").hit then time.slow(0.25, seconds(2)) end
  if hero.hurt then time.freeze(seconds(0.1)) end
end
```

`time.slow(0.25, seconds(2))` is two seconds at a quarter speed, then back to normal. `time.freeze(seconds(0.1))` stops the world for a tenth of a second, the pause that makes a hit feel heavy. And `time.ago(seconds(1))` is a picture of the world one second ago, like a forecast pointing backwards: `time.ago(seconds(1)).hero.x` is where the hero was, which is all a ghost that follows them needs.

That is Part One of Section 19 of the handbook in one lesson. Read that section for the rest: replays you can save and share, tests that play your game, planning enemies, debugging backwards, rooms and watchers online, and the Forge that makes it fast.

## 34. Part Two: A Million Things at Once

Bullets, sparks and coins come in thousands. Make a crowd of them with one word:

```tiberscript
bullet = archetype
  init(x, y, vx, vy) = function
    this.x = x
    this.y = y
    this.vx = vx
    this.vy = vy
    this.life = seconds(2)
  end
end

bullets = swarm of bullet
```

Spawn them, move them all, and let them hit things:

```tiberscript
fixedUpdate() = function
  if action("fire").held then
    every(4) do
      bullets.spawn(player.x, player.y, 4, 0)
    end
  end

  bullets.each do (b)
    b.x += b.vx
    b.y += b.vy
    b.life -= 1
    if b.life <= 0 then b.despawn() end
  end

  bullets.each touching "enemy" do (b, e)
    e.takeDamage(1)
    b.despawn()
  end
end

update() = function
  bullets.draw(circle = 2, color = "white")
end
```

A swarm keeps every `x` in one block of numbers and every `y` in another, instead of a separate object for each bullet. That is about a quarter of the memory, no garbage collector stutter when bullets come and go, and a snapshot the Timeline from Lesson 33 takes in a third of a millisecond, so you can rewind a whole bullet storm.

`where` picks out part of any list or swarm, right in the loop:

```tiberscript
for e in enemies where e.hp < 10 do
  e.flee()
end
```

## 35. Part Three: The Forge, and JavaScript

You do not write anything new for this one. When you press Run, the Forge compiles your game to JavaScript, a function at a time as it is first called, and keeps what it compiled for the next Run. On the benchmarks it runs 67 to 492 times faster than 1.1, a heavy frame, like 5,000 particles or a flood fill, up to 345 times faster, and a real game's frame, six thousand lines of platform game, takes 0.03 ms instead of 0.58. Pressing Run again while you work is about four times quicker than on 1.1, because the browser keeps the game's machine code from the Run before, and even the first Run is quick: the editor compiles your files in the moments you are not typing.

What you will notice is what happens when you save while the game is running. Try it: start a fight, then change the `2` in `step()` to a `4`.

```tiberscript
slime = archetype
  init(x) = function
    this.x = x
    this.speed = 1.5
  end

  step() = function
    this.x += this.speed * 2
  end
end
```

Save, and every slime on the screen speeds up, in the middle of the fight. The game does not start over. Only the functions you changed are compiled again, in a few milliseconds. A number changed in `init()` is used by the slimes made from then on; the ones already alive keep the values they have.

Because the Forge writes JavaScript, JavaScript is not a stranger to TiberScript. Write `javascript` on a line of its own, and everything down to `end` is JavaScript in full. The editor even colors it as JavaScript:

```tiberscript
points = 0

javascript
  let streak = 0

  function addPoints(n = 10) {
    streak += 1
    points += n * streak
    return points
  }

  class Bag {
    constructor() { this.items = [] }
    add(item) { this.items.push(item); return this.items.length }
  }

  const shout = (text) => text.toUpperCase() + "!"
end

init() = function
  addPoints()
  addPoints(5)
  bag = new Bag()
  bag.add("key")
  cheer = shout("nice")
end
```

Look at what just happened, because nothing had to be declared to make it work:

* `addPoints()`, `Bag` and `shout()` were written in JavaScript, and TiberScript calls them like its own.
* `points` is a TiberScript variable, and the JavaScript changes it by name. After `init()` it is `20`.
* `streak` is a JavaScript `let`, and TiberScript could read it or change it by name too. It is one variable, not two copies.
* Anything the page has is there as well: `Math`, `Date`, `navigator`, `fetch`.

A block can also sit inside a function, where it sees the function's own locals and can give a value back:

```tiberscript
copyScore(score) = function
  javascript
    navigator.clipboard.writeText("Score: " + score)
  end
end

highest(list) = function
  local top = javascript
    return Math.max(...list)
  end
  return top
end
```

Keep the ones that reach outside your game for `update()`. The Timeline cannot rewind what JavaScript does out there.

For one function, there is a shorter way. Put `javascript` in front of it, and its body is JavaScript:

```tiberscript
javascript sortHigh(list) = function
  return list.slice().sort((a, b) => b - a)
end

init() = function
  best = sortHigh([12, 99, 3, 47])
  today = js.new("Date").getDay()
  biggest = js.Math.max(3, 9, 4)
end
```

`sortHigh()` is called like any TiberScript function, and its list comes back as a TiberScript list. `js` is the whole page from plain TiberScript: `js.new("Date")`, `js.Math`, `new js.Map()`. Things the web answers later are promises, and a sequence can wait for one:

```tiberscript
loadNotes() = function
  sequence do
    local text = wait(js.fetch("notes.txt"))
    if text != null then print(text.length + " characters") end
  end
end
```

`js.then(promise) do (value, error)` does the same without a sequence, `js.callback(fn)` lets JavaScript call your function at any time (it runs at the start of the next tick), and `js.listen(js.window, "keydown") do (e)` hears the page's events. Section 19.19 of the handbook has every call.

## 36. Part Three, Continued: Fast (WebAssembly)

Put `fast` in front of a function that only does arithmetic:

```tiberscript
fast falloff(d, radius) = function
  local t = clamp(1 - d / radius, 0, 1)
  return t * t * (3 - 2 * t)
end
```

The Forge compiles it to WebAssembly instead of JavaScript, and calls it the same way. `fast` code may use numbers, booleans, `local`, `if`, loops, math, other `fast` functions, functions written with `javascript`, buffers (below), and a swarm member's fields and `despawn()`. If it draws, plays a sound, or makes a list, the checker tells you the line.

It pays most on a crowd:

```tiberscript
fixedUpdate() = function
  sparks.each fast do (s)
    s.vy -= 0.05
    s.x += s.vx
    s.y += s.vy
    s.life -= 1
    if s.life <= 0 then s.despawn() end
  end
end
```

`each fast` runs the whole loop in WebAssembly over the swarm's columns of numbers, where they are. On 100,000 sparks it takes about 0.15 ms, where the same loop run as ordinary code takes about 2. An `each` whose block works on the member's numbers alone runs this way even without the word: the Forge sees it can, and runs it as written whenever it cannot, so the answer is the same either way. And because `fast` code uses TiberScript's own `sin`, `cos` and the rest, what it works out is exactly the same on a phone as on your laptop, so a replay of it plays the same on both.

`approach()`, `remap()`, `wrap()` and a value chosen by a condition (`1 if s.life > 10 else 0`) work in `fast` code too.

Use `fast` where the numbers are. Everywhere else, the Forge's JavaScript is already the fastest thing the browser has.

**Buffers: numbers every language shares.** A swarm is for things. For a plain field of numbers, a grid of heights, a map of distances, there is the buffer:

```tiberscript
heat = buffer(100)

fast spread(h) = function
  for i = 0 to h.length - 1 do
    h[i] = (h[i - 1] + h[i] * 2 + h[i + 1]) * 0.245
  end
  return h.length
end

javascript
  function hottest(list) {
    return Math.max(...list)
  }
end

init() = function
  heat[50] = 100
end

fixedUpdate() = function
  spread(heat)
  peak = hottest(heat)
end

draw() = function
  screen.clear()
  for i = 0 to 99 do
    screen.fillRect(-148 + i * 3, -60 + heat[i], 2, heat[i] * 2 + 1, "#FF8040")
  end
end
```

`buffer(100)` is a hundred numbers, all 0, read and written like a list: `heat[50] = 100`. It never grows and holds only numbers, and in return all three languages work on it in the same place, with nothing copied: TiberScript draws from it, the `fast` function spreads the heat as WebAssembly, and the JavaScript function reads the peak. In `fast` code a place outside the buffer reads 0, so the two ends need no `if`. A buffer is part of your game's world, too: rewind, and the heat flows back.

**Looking at the Forge.** `Tiber.forge` tells you what the Forge made of your code, and lets you decide a few things:

```tiberscript
fast square(x) = function
  return x * x
end

init() = function
  became = Tiber.forge.kind(square)
  oneCall = Tiber.forge.time(square, 1000, 7)
  left = Tiber.forge.compileAll()
end
```

`became` is `"webassembly"`. `Tiber.forge.time()` calls a function a thousand times and says how many milliseconds one call takes, so you can measure instead of guessing. `Tiber.forge.compileAll()` compiles every function nobody has called yet, right now: put it on your loading screen and nothing is left to compile in the middle of a fight. `Tiber.forge.useWasm = false` runs your `fast` code as JavaScript, with exactly the same answers, to compare. And `Tiber.forge.javascript(fn)` and `Tiber.forge.wasm(fn)` show you the very code the Forge wrote.

If you already have WebAssembly from somewhere else, made from C or Rust, `wasm.module(bytes)` starts it and its functions are called like any others. Sections 19.23 to 19.25 of the handbook have all of it.

Not sure which function to make `fast`? Ask. `Tiber.forge.profile = true` times every call from then on, and `Tiber.forge.hot(3)` is the three functions that took the most, each with its `name`, its `ms` and its `calls`. The first is usually `update` (its time counts everything it calls), and the one to work on is the next one down. Turn it off again when you know.


TiberScript for the game, JavaScript when you want the web, WebAssembly where the numbers are, and all three working on the same values: that is Part Three, the Forge, whole.

## 37. Part Four: The Graphics Chip

Your computer has a second processor, the graphics chip, made of thousands of small cores that all run one short program at once, one for every pixel. Put `shader` in front of a function and TiberScript writes it for that chip:

```tiberscript
shader sunset(u, v) = function
  return mix(color(1, 0.55, 0.25), color(0.15, 0.1, 0.4), v)
end

draw() = function
  screen.shade(sunset, 0, 0, screen.width, screen.height)
end
```

A shader is asked one question for every pixel: what color are you? `u` is how far across the area the pixel is, from 0 at the left to 1 at the right, and `v` how far up, from 0 at the bottom to 1 at the top. It answers with `color(r, g, b)`, each part from 0 to 1, or with a number for a grey. `screen.shade(shader, x, y, width, height)` fills that rectangle of the world with it. Here the sky is orange at the bottom and deep blue at the top, two million pixels a frame on a big screen, in about a millisecond.

Make it move by giving it a number of your own after the rectangle:

```tiberscript
shader waves(u, v, t) = function
  local d = distance(u, v, 0.5, 0.5)
  local k = 0.5 + 0.5 * sin(d * 40 - t)
  return mix(color(0, 0.1, 0.3), color(0.3, 0.8, 1), k)
end

init() = function
  clock = 0
end

update() = function
  clock += 1
end

draw() = function
  screen.shade(waves, 0, 0, screen.width, screen.height, clock / 8)
end
```

Every parameter after `u` and `v` is yours to give, up to 16. A shader cannot see your game's variables, only what you pass it, and that is what makes it fast.

Now the best part. `screen.shader(shader)` runs over the whole screen, and `pixel(u, v)` inside it is the color already drawn there. Read a pixel a little to the side, and the picture bends:

```tiberscript
shader wobble(u, v, t) = function
  return pixel(u + sin(v * 30 + t) * 0.01, v)
end

init() = function
  clock = 0
end

update() = function
  clock += 1
end

draw() = function
  screen.clear("#203040")
  screen.fillCircle(mouse.x, mouse.y, 20, "#FFD050")
  screen.text("UNDER WATER", 0, 60, 16, "#FFFFFF")
  screen.shader(wobble, clock / 6)
end
```

That is water, heat haze, a dream, a shockwave or a black hole, in three lines.

A shader uses numbers, true and false, colors and their `.r .g .b .a`, `local`, `if`, loops, the math you know, `noise(x, y)` for clouds and fire (it is the same every frame, so use it instead of `random()`), `hsl()`, `mix()`, `smoothstep()` and other shaders. If you write something the chip cannot do, like `print()` or a list, the Forge tells you the line when the game loads.

Two more things:

* Call a shader from your ordinary code and it runs on the processor and gives you its color: `sunset(0.5, 0.5).r`. That means you can `test` a shader, and a computer with no graphics chip still shows your game, with the same colors.
* `stars.drawPoints(1.5, "#C8D8FF")` hands a whole swarm to the graphics chip and draws every member as a point at once: a million stars in about 5 ms, 28 times faster than drawing them one by one.
* `stars.moveOnChip(drift, clock)` has the chip move them too: a shader whose first parameters are the swarm's fields, `shader drift(x, y, vx, vy, t)`, returning their new values as a `color(...)`. It pays when each member's math is heavy, like several layers of `noise()`; for a simple step, `each fast` is quicker. Section 25 of the handbook.

A shader can read more than the picture under it:

```tiberscript
shader trail(u, v, cx, cy) = function
  local p = previous(u, v)
  local g = max(0, 1 - distance(u, v, cx, cy) * 20)
  return color(p.r * 0.94 + g * 0.3, p.g * 0.94 + g * 0.6, p.b * 0.95 + g, max(p.a * 0.94, g))
end

shader clouds(u, v, t) = function
  local f = fbm(u * 4 + t * 0.05, v * 3, 0.55, 5)
  return mix(color(0.05, 0.07, 0.2), color(0.55, 0.6, 0.85), f)
end
```

`previous(u, v)` is what the same shader drew last frame, so fading it and adding a glow leaves a trail. `fbm()` is layered noise, `voronoi()` is cells, `hash()` a steady random number for each place, and `image(name, u, v)` reads a sprite. `screen.shader(trail, 0.5, 0.5, blend = "add")` adds it on top as light; `blend` also takes `multiply`, `screen`, `overlay` and more, and `opacity` how much. And `glsl name(u, v) = function` writes a shader in GLSL itself, for one you found elsewhere.

A shader can be handed a whole list of numbers, and draw shapes by how far away they are:

```tiberscript
lamps = [-0.3, 0, 0, 0.1, 0.3, 0]

shader lampGlow(u, v, points, count) = function
  local x = (u - 0.5) * aspect()
  local y = v - 0.5
  local light = 0
  for i = 0 to count - 1 do
    local d = sdCircle(x - points[i * 2], y - points[i * 2 + 1], 0.02)
    light += 0.003 / max(0.003, d * d)
  end
  return color(light, light * 0.8, light * 0.4, min(1, light))
end

draw() = function
  screen.clear("#05060C")
  screen.shade(lampGlow, 0, 0, screen.width, screen.height, lamps, 3, quality = 0.5)
end
```

`points[i]` in a shader reads a list the game gives it, a list or a buffer, so three lamps or three hundred are one parameter. `sdCircle()` is the distance to a circle, and a distance is a glow, an outline or a soft edge with one more line; `sdBox()` and `sdLine()` do the same for a box and a line. `quality = 0.5` works the shader out for a quarter of the pixels and stretches it smooth, which nobody sees on a glow and which costs a quarter. `Tiber.shaderScale = "auto"` lets the engine pick the quality of every shader by how well the game is keeping up.

Why is this the fourth part of the biggest update? Because the Forge, which already writes your game as JavaScript and your `fast` code as WebAssembly, now writes your shaders in the graphics chip's language too: one language for all three. A shader keeps nothing, so rewinding the game rewinds the picture with it. And Part Two's million swarm members are finally cheap to see, not only to keep and move. That is Section 19, Part Four of the handbook.

## 38. Part Five: HTML, Powered by the Whole Forge

A shop, a settings screen, a bag of items: the web has been building screens like these for thirty years, with HTML. This lesson writes one inside your game's own file, in TiberScript.

```tiberscript
html shop
  <style>
    .shop { position: absolute; right: 4%; top: 10%; padding: 12px; border-radius: 12px; background: rgba(20, 30, 70, 0.9); color: white }
    button { display: block; width: 100%; margin: 4px 0 }
  </style>
  <div class="shop">
    <p>Gold: $(gold)</p>
    <div each="item in items">
      <button do="buy(item)" disabled="$(gold < item.price)">$(item.name): $(item.price)</button>
    </div>
    <p show="owned.length > 0">You own: $(string.join(owned, ', '))</p>
    <label>Volume <input type="range" min="0" max="1" step="0.1" bind="volume"></label>
  </div>
end

buy(item) = function
  gold -= item.price
  owned.push(item.name)
end

init() = function
  gold = 30
  owned = []
  volume = 0.5
  items = [data(name = "Ring", price = 5), data(name = "Sword", price = 20), data(name = "Bow", price = 8)]
  html.show("shop")
end

update() = function
  if key("g").hit then gold += 10 end
end
```

Run it, buy the Ring and the Bow, then press G.

* `html shop ... end` is a page named `shop`, written as the web writes HTML, with its own CSS. `html.show("shop")` shows it over the game, the same call as for any page of Section 9 of the handbook.
* `$(gold)` is the game's `gold`, always as it is now. Buy something and the number goes down; press G and it goes up, with no code that tells the page.
* `do="buy(item)"` runs `buy()` with the item of that button when it is clicked.
* `disabled="$(gold < item.price)"` turns the button off while you cannot pay, and back on when you can.
* `each="item in items"` makes the button once for every item, and `show=` shows the line of what you own only once you own something.
* `bind="volume"` keeps the slider and `volume` equal, both ways: move the slider and `volume` changes; set `volume` in the code and the slider moves.

Inside an attribute, a text is written with single quotes, `', '`, since the attribute is in double quotes. Every piece of TiberScript in the page is compiled by the Forge with the rest of your game, and a page that is not shown costs nothing.

**Try it.** Add an item that costs 100 gold, and a line `<p show="gold >= 100">Rich!</p>`. Then press G until it shows. That is Section 19, Part Five of the handbook.

## 39. Part Six: Change the Past

The biggest part of all. Lesson 33 gave your game a memory. This lesson lets you change what is in it.

```tiberscript
init() = function
  time.keep(seconds(10))
  bind("right", "arrowRight")
  bind("wall", "w")
  bind("undo", "z")
  bind("compare", "c")
  hero = register("hero", -100, 0)
  wall = false
end

fixedUpdate() = function
  if action("right").held then hero.x += 2 end
  if wall and hero.x > 0 then hero.x = 0 end
  if action("wall").hit then
    retro seconds(2) do
      wall = true
    end
  end
  if action("undo").hit then time.undoChange() end
  if action("compare").hit then time.showOldPast(true) end
end

draw() = function
  screen.clear()
  if wall then screen.fillRect(8, 0, 4, 60, "#C084FC") end
  screen.fillCircle(hero.x, hero.y, 6, "#8CC0FF")
  local y = 80
  for line in time.changes() do
    screen.text(line, 0, y, 7, "#FFD750")
    y -= 10
  end
end
```

Hold right until the hero is well past the middle, then press W.

* `retro seconds(2) do ... end` goes back two seconds, puts the wall up there, and plays the two seconds since again, with the same keys you pressed. The hero did not walk through a wall that appeared: the wall was there, and the hero stopped at it, two seconds ago.
* `time.changes()` says what came out different, as lines like `hero.x  142 → 0`, and `time.change` has the rest: how many ticks were played again, how long that took, and how long ago the first difference is.
* Z is `time.undoChange()`: back to what had happened, exactly.
* C is `time.showOldPast(true)`: what had happened, drawn faintly over the new present, so you see both heroes.

Now the part that feels like magic. Keep the game running, press Z if the wall is up, hold right for a while, then change `hero.x += 2` to `hero.x += 3` in the code, and save. The game does not restart. The time it kept is played again with the new code, so the hero is where three a tick would have taken it, and a card at the top says THE PAST CHANGED, with KEEP, UNDO and COMPARE. Moving a tunable's slider does the same. A game that wants a saved change to work from now on only says `time.changeThePast = false`.

The rules are the rules of Lesson 33: the world changes in `fixedUpdate()`, input comes through actions, randomness is seeded. A game that follows them can change its past exactly. That is Section 19, Part Six of the handbook.

## 40. Light and Effects

Turn the lights off:

```tiberscript
init() = function
  lighting.ambient = 0.15
end

draw() = function
  screen.clear("#556070")
  screen.fillRect(0, -60, 300, 20, "#8A7A62")
  screen.light(mouse.x, mouse.y, 90, "#FFD080")
end
```

`lighting.ambient` is how bright the dark is: 1 (where it starts) is no dark, 0 is black. `screen.light(x, y, radius, color)` is a light, for this frame only, drawn like anything else. At the end of `draw()` the engine darkens everything except where the lights are. Draw things in their full colors and let the dark take them down.

Now give the light something to hide behind:

```tiberscript
init() = function
  lighting.ambient = 0.1
end

draw() = function
  screen.clear("#6A6470")
  screen.fillRect(-40, 0, 20, 60, "#B0A080")
  lighting.block(-40, 0, 20, 60)
  screen.light(mouse.x, mouse.y, 160, "#FFFFFF")
end
```

`lighting.block()` takes a box, an object, a whole tilemap or a swarm, and every light throws its shadow behind it. Move the mouse around the pillar and watch the shadow swing.

More things, each one line:

* `screen.spotlight(x, y, radius, angle, width, color)` is a flashlight's cone, the angle in radians like `atan2()`.
* `flicker = 0.4` at the end of any light makes it waver like a torch.
* `sparks.lights(10, "#FFB050")` puts a light on every member of a swarm.
* `lighting.blockCircle(x, y, radius)` is a round thing in the light's way, a rock or a barrel, with a round shadow.
* `lighting.softness = 0.5` makes shadow edges soft, like a lamp with a shade (0 is sharp).
* `screen.sunlight(angle, strength, color)` is the sun: light over the whole world, all one way, so every shadow points the same way. Turn the angle slowly and the day goes by.
* `screen.glow(x, y, radius, color)` is a glow that lights nothing and the dark never covers: neon, a monster's eyes, an item to pick up.

And the light is something your game can ask about. `lighting.at(x, y)` is how bright a place is, from 0 to 1, shadows counted, and `lighting.dark(x, y)` is true in the dark: that is a whole stealth game's rule in one line. `lighting.colorAt(x, y)` is the color of the light there, as `"#RRGGBB"`.

Draw the dark before your HUD so the score stays bright:

```tiberscript
init() = function
  lighting.ambient = 0.15
  effects.vignette = 0.6
end

draw() = function
  screen.clear("#505868")
  screen.light(0, 0, 80, "#FFFFFF")
  lighting.draw()
  screen.text("SCORE 120", 0, 80, 10, "#FFD750")
end
```

`lighting.draw()` puts the dark on right there, and what comes after it stays lit. `effects` are a finish on the whole frame: `effects.crt`, `.bloom`, `.scanlines`, `.vignette`, `.chromatic`, `.grain`, `.pixelate` and `.tint`, each from 0 to 1.

And your own shaders can be effects too. `heat = effects.filter(heatWave, 0)` puts a shader on the finished frame every frame, stacked after any filters before it, like layers in a photo editor: `heat.strength = 0.5` mixes it at half, `heat.on = false` skips it, `heat.set(clock)` gives it new values, and `heat.remove()` takes it off. Handbook 23.11.

**Mirrors and water.** Two more lines finish a scene. `lighting.mirror(x1, y1, x2, y2)` is a mirror: light does not pass it, and every light that reaches it is thrown back, so a lantern in front of a mirror lights the room twice. `screen.water(x, y, w, h)` is a lake: it shows what is above it upside down, rippling, tinted and fading with depth. `screen.mirror(x, y, w, h)` is the same picture with no ripples, for a mirror on a wall or a polished floor.

```tiberscript
draw() = function
  screen.clear("#1B2238")
  screen.fillRect(hero.x, hero.y, 14, 22, "#FFD750")
  screen.light(hero.x, hero.y, 90, "#FFD9A0")
  lighting.mirror(120, -40, 120, 60)
  screen.water(0, -60, 400, 80)
  lighting.draw()
end
```

Draw the world, then the water, then the light: the light falls on the reflection too.

**A lens and a shockwave.** Two more lines bend what is already drawn. `screen.lens(x, y, radius)` pulls the picture in toward a point, like a black hole bending the stars behind it, and `screen.shockwave(x, y, radius, width, push, color)` is a ring that pushes the picture outward and lights it: grow its radius every frame and it is the wave of an explosion.

```tiberscript
init() = function
  wave = 0
end

update() = function
  wave = min(1, wave + 0.02)
  if keyboard.press.SPACE then wave = 0 end
end

draw() = function
  screen.clear("#0B0A20")
  for i = 0 to 30 do
    screen.fillCircle((i * 41) % 360 - 180, (i * 59) % 200 - 100, 2, "#C8D8FF")
  end
  screen.lens(60, 0, 50, 1.5, core = 0.2, ring = "#E0A8FF")
  if wave < 1 then screen.shockwave(0, 0, wave * 150, 20, 10 * (1 - wave), "#4DB3FF") end
end
```

Both copy the picture onto itself, so they are as quick as a rectangle in every browser. Section 23.14 of the handbook has every value they take.


A light is drawn from where things are, every frame, and nothing about it is kept. Rewind your game and the lantern goes back with the hero. Replay it, forecast it, roll it back online: the light is always right, and you never wrote a line for it. Light has its own section, Section 23 of the handbook.

## 41. Physics That Rewinds

Make a crate a real body, and give it a floor:

```tiberscript
init() = function
  floor = register("floor", 0, -80)
  physics.add(floor, "box", 300, 20, fixed = true)
  crate = register("crate", 0, 60)
  physics.add(crate, "box", 16, 16)
  ball = register("ball", -40, 80)
  physics.add(ball, "circle", 6, bounce = 0.8)
end

draw() = function
  screen.clear("#203040")
  screen.fillRect(floor.x, floor.y, 300, 20, "#556070")
  screen.fillRect(crate.x, crate.y, 16, 16, "#C08850")
  screen.fillCircle(ball.x, ball.y, 6, "#78DC8C")
end
```

The crate falls and lands, the ball bounces lower each time and then rolls. `fixed = true` is a body that never moves; `bounce =`, `friction =` and `mass =` say what the others are made of. `physics.push(ball, 3, 0)` shoves it, `ball.grounded` is true while it stands on something, and `physics.touching(ball, crate)` says whether two touch.

Now give the game a memory, and a rewind button:

```tiberscript
init() = function
  time.keep(seconds(10))
  floor = register("floor", 0, -80)
  physics.add(floor, "box", 300, 20, fixed = true)
  ball = register("ball", -100, 40)
  physics.add(ball, "circle", 6, bounce = 0.7)
  ball.vx = 3
end

fixedUpdate() = function
  if key("r").held then
    time.rewind(1)
    return 0
  end
end

draw() = function
  screen.clear("#203040")
  screen.fillRect(floor.x, floor.y, 300, 20, "#556070")
  forecast 60 each (future) do
    screen.fillCircle(future.ball.x, future.ball.y, 1, "#FFD750")
  end
  screen.fillCircle(ball.x, ball.y, 6, "#78DC8C")
end
```

Hold R and the ball flies back along its bounces. The yellow dots are where it really will go, bounces and all. Nothing here was written for the physics: it keeps everything on the objects themselves, so the Timeline from Lesson 33 keeps it too, exactly, and every machine gets the same bounces. That is Section 24 of the handbook.

## 42. Achievements, Stats and Leaderboards: platform

Every finished game wants achievements. `platform` gives them to you, the same in every language microStudio runs:

```tiberscript
init() = function
  platform.define("first_coin", "First Coin", "Pick up a coin")
  coins = 0
end

fixedUpdate() = function
  if key("c").hit then
    coins += 1
    platform.unlock("first_coin")
    platform.setStat("coins", coins)
    platform.submitScore("most_coins", coins)
  end
end
```

Play it in the editor and press C: a banner slides down saying ACHIEVEMENT UNLOCKED. In a browser everything is kept on the player's computer. Export the same game for Windows, Linux or macOS with a Steam App ID typed into the project's Settings, and the very same lines unlock a Steam achievement, set a Steam stat and post to a Steam leaderboard. `platform.cloudSave(name, value)` and `platform.cloudLoad(name)` keep a save that follows the player, and `platform.scores(board)` is a leaderboard's top scores. Section 22 of the handbook has every call, Epic Online Services, and why the consoles' SDKs are their own story.

## 43. The Language, Rounded Out

A handful of small things make everyday code shorter and clearer. Here they all are in one game:

```tiberscript
const MAX_HP = 30

monster = archetype
  init(name, hp = MAX_HP, speed = 1) = function
    this.name = name
    this.hp = hp
    this.speed = speed
  end
end

danger(m) = function
  match m.hp
    case 0 then return "gone"
    case 1 to 10 then return "weak"
    else return "strong"
  end
end

init() = function
  slime = new monster("slime")
  bat = new monster("bat", speed = 3, hp = 8)
  label = "fast" if bat.speed > 2 else "slow"
  nickname = null
  shown = nickname otherwise slime.name
  inventory = ["key"]
  if "key" in inventory then print("you can open the door") end
  for field, value in data(hp = bat.hp, speed = bat.speed) do
    print(field + ": " + value)
  end
  print(danger(bat))
end
```

`hp = MAX_HP` gives a parameter a value of its own for when a call leaves it out, and `new monster("bat", speed = 3, hp = 8)` gives values by name, in any order. `match` picks one case (`case 1 to 10` is a range), `const` is a name that keeps its value, `"fast" if ... else "slow"` chooses a value, `otherwise` replaces a `null`, `in` asks whether a list holds something, and a loop can name two things at once. In a sequence, `wait until player.x > 200` waits for a condition. Section 1 of the handbook has each one in full.

## 44. Game Feel and Forgiving Input

```tiberscript
init() = function
  bind("jump", "space")
  bind("jump", controllers.button("south"))
  bind("left", "arrowLeft")
  bind("left", controllers.button("left"))
  bind("right", "arrowRight")
  bind("right", controllers.button("right"))
  bind("up", "arrowUp")
  bind("up", controllers.button("up"))
  bind("down", "arrowDown")
  bind("down", controllers.button("down"))
  hero2 = data(x = 0, y = 0, vx = 0, vy = 0, grounded = true)
end

fixedUpdate() = function
  local stick = actionVector("left", "right", "down", "up")
  hero2.vx = approach(hero2.vx, stick.x * 4, 0.4)
  if action("jump").within(8) and hero2.grounded then
    action("jump").consume()
    hero2.vy = 7
  end
  if cooldown("sparkle", 30) and chance(0.2) then
    print(weightedChoice(["dust", "gem"], [9, 1]))
  end
end
```

`actionVector()` turns four actions into a direction that is never longer than 1, so a stick steers round a smooth circle and a half push is half the speed. `within(8)` keeps a jump pressed a moment too early until the hero can jump, and `consume()` makes one press one jump. `approach()` eases a speed toward its target, `cooldown()` says when something is ready again and rewinds with the world, `chance()` and `weightedChoice()` roll the game's own seeded dice. There is more in Section 2: `remap()`, `wrap()`, `ease()`, `grid()`, `inspect()`, and `tr()` for a game in more than one language.

## 45. Colors You Can Work With

Until now a color was something you typed: `"#FF8800"`. Type enough of them and a game has thirty colors that almost match. `colors` makes one color from another, so you choose a few and work the rest out.

```tiberscript
init() = function
  team = "#3A7BD5"
  hp = 30
end

draw() = function
  screen.clear(colors.darken(team, 0.7))
  screen.rect(0, 40, 120, 24, team)
  screen.rect(0, 10, 120, 24, colors.lighten(team, 0.3))
  local bar = colors.mix("#FF5050", "#78DC8C", hp / 100)
  screen.rect(0, -40, 100, 10, bar)
end
```

Read it from the top. The background is the team's blue made much darker. The second band is the same blue, 30 percent of the way to white. And the bar is a color between red and green: `colors.mix(a, b, amount)` gives `a` at 0, `b` at 1, and everything in between. With 30 health out of 100 it is mostly red. Change `team` to `"#D53A7B"` and the whole screen follows, because nothing else was typed.

The ones you will use most:

* `colors.mix(a, b, amount)`: the color between two
* `colors.lighten(color, amount)` and `colors.darken(color, amount)`: toward white, toward black
* `colors.fade(color, alpha)`: the same color, see through (0 is gone, 1 is solid)
* `colors.hsl(hue, strength, lightness)`: a color from a place on the color wheel

`colors.hsl()` is the one for colors that move. The hue is an angle: 0 is red, 120 green, 240 blue, and 360 is red again. Turn it a little each update and you have a rainbow.

```tiberscript
init() = function
  turn = 0
end

update() = function
  turn = (turn + 3) % 360
end

draw() = function
  screen.clear("#101020")
  for i = 0 to 7 do
    screen.circle(-70 + i * 20, 0, 8, colors.hsl(turn + i * 45, 0.8, 0.6))
  end
end
```

One more that saves a real problem: text on a color you worked out. `colors.brightness(color)` is how bright a color looks, from 0 to 1. Above 0.6, write on it in black. Below, in white.

```tiberscript
draw() = function
  screen.clear("#101020")
  local box = colors.hsl(50, 0.9, 0.6)
  local ink = "#FFFFFF"
  if colors.brightness(box) > 0.6 then
    ink = "#000000"
  end
  screen.rect(0, 0, 120, 30, box)
  screen.text("READY", 0, 0, 12, ink)
end
```

**Try it.** Make the bar of the first example pulse when health is under 20: mix its color with white by an amount that goes up and down with the frame count.

There is more in the Handbook's Section 2: `colors.rgb()`, `colors.parts()`, and `colors.valid()`.

## 46. Transitions: From One Screen to the Next

Your game has a title and a level. Press a key and the title is gone and the level is there, in one frame. It works, and it feels like a mistake. What is missing is the moment in between, and it is one line.

```tiberscript
init() = function
  room = "title"
end

update() = function
  if room == "title" and key("space").hit and not transition.active then
    transition.fade(0.4) do
      room = "play"
    end
  end
end

draw() = function
  if room == "title" then
    screen.clear("#1C1C3E")
  else
    screen.clear("#103020")
  end
  screen.text(room, 0, 0, 16, "#FFFFFF")
end
```

`transition.fade(0.4)` does three things in order. It covers the screen with black over 0.4 seconds. When the screen is fully covered, it runs the block between `do` and `end`, once: that is where the room changes, and nobody sees it change. Then it uncovers the screen over 0.4 seconds more. The engine draws all of it, over everything, so `draw()` stays as it was.

Two details in that example matter. `not transition.active` stops a second press from starting a second transition. And the room is changed inside the block, not before it: change it before, and the level would be on the screen while it is still fading out.

There are four transitions, and they all work the same way:

* `transition.fade(seconds)`: to a color and back
* `transition.wipe(seconds, "left")`: a curtain from a side: `"left"`, `"right"`, `"top"` or `"bottom"`
* `transition.iris(seconds, x, y)`: a circle that closes on a point and opens from it
* `transition.blinds(seconds)`: strips that close and open

The iris is the one to close on your hero when a level ends:

```tiberscript
init() = function
  level = 1
  hero = data(x = 40, y = -20)
end

update() = function
  if key("n").hit and not transition.active then
    transition.iris(0.5, hero.x, hero.y, hold = 0.2) do
      level += 1
      hero.x = -100
    end
  end
  if transition.phase != "out" then
    hero.x += 1
  end
end

draw() = function
  screen.clear("#203040")
  screen.circle(hero.x, hero.y, 6, "#FFD750")
  screen.text("level " + level, 0, 80, 10, "#FFFFFF")
end
```

`hold = 0.2` keeps the screen covered for a fifth of a second before it opens: a breath. `color = "#FFFFFF"` would make it white, and `back = 1` would make the way back take a full second whatever the way out took. And the hero stops walking while the circle closes, because the game asks: `transition.phase` is `"out"` while the screen is being covered, `"hold"` while it stays covered, `"back"` while it opens, and `""` when nothing is running.

One more form is worth knowing: 0 seconds on the way out. The screen is covered at once and only the way back is seen, which is a fade in. `transition.iris(0, hero.x, hero.y, back = 0.6)` at the start of a level opens it on its hero.

**Try it.** Give the first example a way back: in the level, the escape key wipes to the title from the right.

The Handbook has the rest in Section 5: `transition.covered`, `transition.kind` and `transition.stop()`.

## 47. Springs

In Lesson 44 you met `approach()`, which takes a number to its target and stops there. Real things do not stop like that. They arrive, go a little too far, and come back. That small overshoot is most of what people mean when they say a game feels good, and `springTo()` is all of it.

```tiberscript
init() = function
  button = data(size = 1)
end

update() = function
  if key("space").hit then
    button.size = 1.5
  end
  springTo(button, "size", 1)
end

draw() = function
  screen.clear("#101020")
  screen.rect(0, 0, 60 * button.size, 24 * button.size, "#3C4A78")
  screen.text("PLAY", 0, 0, 10 * button.size, "#FFFFFF")
end
```

Press space. The button jumps to one and a half times its size, and `springTo(button, "size", 1)` pulls it back to 1: under it for a moment, then up, then still. You call it once every update, with the object, the name of the field as text, and where the field should end up.

Two options change the character of a spring:

* `stiffness` (0.2 when you leave it out): how hard it pulls. Higher is snappier.
* `damping` (0.75 when you leave it out): how much speed it keeps. Higher swings longer, lower settles at once.

```tiberscript
init() = function
  panel = data(x = -200)
  coin = data(y = 0)
end

update() = function
  springTo(panel, "x", 0, stiffness = 0.08, damping = 0.85)
  if key("c").hit then
    coin.ySpeed = 6
  end
  springTo(coin, "y", 0, stiffness = 0.3, damping = 0.6)
end

draw() = function
  screen.clear("#101020")
  screen.rect(panel.x, 60, 160, 30, "#262E4E")
  screen.circle(0, coin.y - 20, 8, "#FFD750")
end
```

The panel slides in softly and settles. The coin shows the second way to use a spring: do not move the value, kick its speed. `springTo()` keeps the speed of a field in a second field with `Speed` on the end, so `coin.ySpeed = 6` throws the coin up and the spring brings it back down.

Because a spring is only those two numbers on your object, it comes along with everything else: `save.game()` saves it in the middle of a bounce, and a rewind runs it backwards.

**Try it.** Give the hero of any earlier lesson a `squash` field that starts at 1, set it to 0.6 when the hero lands, spring it back to 1, and draw the hero that much shorter and that much wider.

Use springs for what the eye follows: sizes, tilts, a camera's zoom, a score counting up. The hero's own position belongs to physics.

## 48. watch(): Seeing What Your Game Thinks

A bug in a game is almost always a number that is not what you think it is. `print()` can tell you, sixty times a second, in a console that scrolls faster than you can read. `watch()` puts the number on the game's own screen, in a corner, on one line that changes in place.

```tiberscript
init() = function
  hero = data(x = 0, vx = 2)
end

update() = function
  hero.x += hero.vx
  if hero.x > 100 or hero.x < -100 then
    hero.vx = -hero.vx
  end
  watch("x", hero.x)
  watch("speed", hero.vx)
  if hero.vx < 0 then
    watch("going left")
  end
end

draw() = function
  screen.clear("#101020")
  screen.circle(hero.x, 0, 6, "#FFD750")
end
```

Run it and look at the top left corner: `x` counts up and down, `speed` flips between 2 and -2, and a third line, `going left`, is there only while the hero goes left. That last one is a `watch()` with a label and no value. Use it to see which branch of your code is running, which is very often the whole question.

Three things to know:

* A line stays for two seconds after its last `watch()`, then goes. You never clear anything.
* You can watch a list or an object: it is shown the way `inspect()` writes it.
* `Tiber.watch = false` hides every line at once, and `Tiber.watch = true` brings them back. Put it on a key while you work, and you do not have to take the calls out to show your game to someone.

**Try it.** Go back to the springs of Lesson 47 and watch `button.sizeSpeed`. You will see the spring's speed swing through zero each time the button turns around.

When you have gone past the tick where something went wrong, remember Lesson 33: with `time.keep()` you can go back to it.

## 49. For Every Player and Every Computer

Two things separate a game that works for you from a game that works for everyone: the player may not be like you, and their computer may not be like yours.

**The player.** For some people a shaking, flashing screen is not excitement. It is nausea, or worse. Their system has a setting that says so, and your game can read it and has a setting of its own:

```tiberscript
init() = function
  if Tiber.prefersLessMotion then
    Tiber.motion = 0.3
  end
end

update() = function
  if key("h").hit then
    shake(6, 12)
    flash("#FFFFFF", 0.6, 8)
  end
  if key("m").hit then
    if Tiber.motion > 0 then
      Tiber.motion = 0
    else
      Tiber.motion = 1
    end
  end
end

draw() = function
  screen.clear("#101020")
  screen.text("motion " + Tiber.motion, 0, 0, 10, "#FFFFFF")
end
```

`Tiber.motion` goes from 0 to 1 and scales every `shake()` and every `flash()` in your whole game. You do not change a single one of them. At 1 they are as you wrote them, at 0.3 a shake of 6 is a shake of 1.8, and at 0 the screen stays still. Put it in your options menu next to the volume.

**The player's language.** One line shows every text your game draws in another language, at the same place and in the same width, with its numbers kept:

```tiberscript
translations = data
  es = data(start = "Empezar")
end

init() = function
  translations.es["LIVES #1"] = "VIDAS #1"
  Tiber.translate = "es"
  lives = 3
end

draw() = function
  screen.clear("#101020")
  screen.text("LIVES $(lives)", 0, 0, 12, "#FFFFFF")
end
```

This draws `VIDAS 3`. `Tiber.translate("japanese")` works the same, `"auto"` is the player's own language, and `"off"` shows the texts as you wrote them. A text with no translation of yours is asked of the browser's own translator where there is one, and what it answers while the game runs in the editor is kept in the project, so the exported game has it everywhere. Section 2 of the handbook has the rest.

**The computer.** Your game's logic is not what slows an old laptop down: a whole game thinks for a few hundredths of a millisecond a frame. Drawing is what costs. So the two most useful lines for an old computer are these:

```tiberscript
init() = function
  Tiber.frameRate = "auto"
  Tiber.renderScale = "auto"
  sparks = 0
end

update() = function
  local wanted = 40
  if system.fps < 45 then
    wanted = 10
  end
  sparks = approach(sparks, wanted, 1)
end

draw() = function
  screen.clear("#101020")
  screen.text("sparks " + sparks, 0, 0, 10, "#FFFFFF")
end
```

`"auto"` lets the engine draw as often and as sharply as this computer can manage, and fewer pixels when it cannot. The rest of the example is the rule to remember: **scale the decoration, never the game.** Forty sparks on a fast computer and ten on a slow one is the same game. Fewer enemies on a slow computer is a different game.

And one thing that is done for you. On the computer microStudio runs on, the controllers of the three console makers and of Valve are read the moment they report, not when the browser gets around to it. `gamepad(0).pace` tells you how often a controller is read, in thousandths of a second: around 15 is the controller's own pace.

**Try it.** Add the PerfMonitor library to one of your games, give it a key with `PerfMonitor.useKey("m")`, press M in it, and read the frame rate, the update time and the draw time. Then draw two hundred circles one by one and look again. Now you know what costs. The monitor measures the screen's own refresh and counts every refresh a picture missed, so a slow computer shows as slow, never as a slower screen.

The whole list is in the Handbook, Section 21.10.

## 50. Buttons That Know Time

`hit` says a button went down. That is enough for a jump. It is not enough for a menu, and it is not enough for a sword.

**A menu that keeps moving.** With `hit`, a player presses down ten times to reach the tenth line. With `repeat`, they hold down and the menu walks: once at the press, again after a short wait, then steadily for as long as the direction is held.

```tiberscript
init() = function
  lines = ["PLAY", "LEVELS", "OPTIONS", "RECORDS", "QUIT"]
  choice = 0
end

update() = function
  if key("arrowDown").repeat or controllers.repeat("down") then
    choice = (choice + 1) % lines.length
  end
  if key("arrowUp").repeat or controllers.repeat("up") then
    choice = (choice + lines.length - 1) % lines.length
  end
end

draw() = function
  screen.clear("#101020")
  for i = 0 to lines.length - 1 do
    local color = "#8890A8"
    if i == choice then color = "#FFD750" end
    screen.text(lines[i], 0, 36 - i * 18, 10, color)
  end
end
```

`controllers.repeat("down")` is the D pad or the left stick of any controller. `key("arrowDown").repeat` is the same for a key. Both wait 24 ticks before they repeat and then repeat every 6, and `controllers.repeat("down", 12, 3)` is quicker.

**One button, three moves.** A tap, a hold and a double tap are three different things a thumb can say with the same button:

```tiberscript
init() = function
  said = "press the left face button"
  power = 0
end

update() = function
  local pad = gamepad(0)
  if pad.tapped("west") then said = "a quick swing" end
  if pad.longPress("west", seconds(0.5)) then said = "a heavy swing" end
  if pad.doubleTap("south") then said = "a second jump, in the air" end
  power = pad.heldFor("east")
  if pad.released("east") then said = "thrown, after " + power + " ticks of winding up" end
end

draw() = function
  screen.clear("#101020")
  screen.text(said, 0, 0, 9, "#FFFFFF")
end
```

`tapped()` is true when the button is let go quickly. `longPress()` is true once, at the moment the hold has lasted long enough. `heldFor()` counts the ticks, for a throw that goes farther the longer it is wound up.

**How hard is a press?** A trigger is not on or off. It says how far it is pulled, and you choose where "pressed" begins:

```tiberscript
init() = function
  gamepad(0).setThreshold("rt", 0.8)
  controllers.curve = 1.5
  x = 0
end

update() = function
  x += (controllers.amount("right") - controllers.amount("left")) * 3
  if gamepad(0).hit("rt") then print("fired: that took a firm pull") end
end

draw() = function
  screen.clear("#101020")
  screen.circle(x, 0, 6, "#FFD750")
end
```

`controllers.amount("right")` is how far the stick leans right, from 0 to 1, so the dot creeps when the stick leans a little and runs when it leans all the way. `controllers.curve = 1.5` makes the middle of every stick finer, which is what aiming wants.

**Say it in words, or let the engine measure.** Every one of these settings also takes a word: `controllers.threshold = "light"`, `controllers.deadzone = "wide"`, `controllers.curve = "aim"`. And `"auto"` has the engine measure the controller in the player's hands: where its buttons sit when nobody presses them, and how far its sticks lean when nobody touches them.

```tiberscript
init() = function
  controllers.feel = "fast"
end

update() = function
  watch("left stick rests at", gamepad(0).drift.left)
end
```

`controllers.feel = "fast"` sets them all at once for an action game: the lightest press counts, and a worn stick still rests at zero. `"precise"` is for aiming, `"relaxed"` forgives a thumb resting on a button, and `"normal"` is how the engine starts. `pad.drift` shows what `"auto"` measured.

**Try it.** Give a menu of yours `repeat` in place of `hit`. Then hold the direction and count how many presses you just saved your player.

The whole list (every name, the touchpad, the back buttons some controllers have) is in the Handbook, Section 7.

## 51. The Settings Screen and the Monitor: Two Libraries

Open the code editor of any TiberScript game, press **Import Libraries**, and pick **Settings**, the library listed first, recommended for every TiberScript 1.2 game. No key opens it until your game picks one, so it never takes a key your game uses. Give it one in your `init()`:

```text
Settings.key = "escape"
Settings.button = "start"
```

Then run the game and press Escape, or START on a controller.

That screen was not written by your game. It is the Settings library, a file of TiberScript that is now part of your project, and the game waits behind it until it closes: it is your game's pause as well. Its pages are PICTURE, SOUND and CONTROLS. In CONTROLS is a CONTROLLER CHECK: press any button on a controller, and it goes through every button, both triggers, both sticks all the way round, how fast your taps get through, and whether the controller drifts or presses by itself when nobody touches it. When your game binds its actions to keys, KEYS lets the player give each action a new key.

What is on it is what only the player can know: how many pictures a second their screen shows, how many pixels their computer can draw without slowing down, how much shaking they like, how loud the music and the sounds should play, how strong rumble should be, how hard they press a button, and how worn their sticks are. What they choose is kept for that game and set again the next time it starts.

So you do not have to write any of it. What you can do is give your own menu one line that opens it:

```tiberscript
init() = function
  Tiber.frameRate = "auto"
  lines = ["PLAY", "SETTINGS"]
  choice = 0
end

update() = function
  if key("arrowDown").repeat or key("arrowUp").repeat or controllers.repeat("down") or controllers.repeat("up") then
    choice = 1 - choice
  end
  if key("enter").hit or controllers.hit("confirm") then
    if choice == 1 then Tiber.settings.open() end
  end
end

draw() = function
  screen.clear("#101020")
  for i = 0 to 1 do
    local color = "#8890A8"
    if i == choice then color = "#FFD750" end
    screen.text(lines[i], 0, 10 - i * 18, 10, color)
  end
end
```

What your `init()` sets is where the screen starts: this game asks for `"auto"` pictures a second, and a player who picks 60 gets 60 from then on.

A menu line of your own can change one setting without opening the screen. `Tiber.settings.next("rumble")` moves rumble to its next choice (full, half, off) and keeps it, and `Tiber.settings.name("rumble")` is the word to show beside it.

**Try it.** Open the settings screen in a game with a controller in your hands, set BUTTON PRESS to hard in CONTROLS, and feel how far a trigger has to go. Then set it to light.

One more library comes with it. **PerfMonitor** shows the frame rate the screen really shows, the slowest frames and the frames dropped, on the key your game gives it with `PerfMonitor.useKey("m")`, and it is the same library in all twelve languages. The settings screen holds the game the way you can hold it yourself, with `Tiber.hold(screen)`.

The whole list is in the Handbook, Sections 21.6, 21.11 and 21.12.

**One line of it is about pictures.** MOMENTS asks the engine to keep pictures, and short clips, of a game's best moments. Your game can ask for the same thing, and say which moments are its best:

```tiberscript
init() = function
  capture.auto = "auto"
  capture.clips = true
  bossHp = 3
end

update() = function
  if key("space").hit and bossHp > 0 then
    bossHp -= 1
    if bossHp == 0 then capture.moment("the boss falls") end
  end
end

draw() = function
  screen.clear("#101020")
  screen.text("boss hp " + bossHp + "   pictures " + capture.count, 0, 0, 10, "#FFFFFF")
end
```

`capture.moment("the boss falls", data(level = 3))` keeps that data with it too, read back as `item.info`, and `capture.draw(item, x, y, w, h, frame)` draws a kept picture, or one picture of a clip, on your own screen: a gallery, or the best moment of a run on the game over screen. `capture.moment()` keeps a picture of the frame the boss falls in, and with clips on, the seconds that led up to it. With `capture.auto` on, the engine finds moments too: a sprite seen for the first time, a sudden burst of particles, shaking and flashes. `"auto"` turns that on only on a computer with the memory for it. Nothing is kept until a game or its player asks. The Handbook has the rest, in Section 26.

## 52. Final Test: Boss Arena

Copy this into a fresh project set to TiberScript. This uses modules, traits, enums, entering blocks, sequences, signals, actions, camera, layers, tween easing, fixed timestep physics, real collision, save data, controllers, and all five parts of the biggest update, in one small game.

```tiberscript
// enemies.tbs
export goblin = archetype
  signal defeated()
  export moveSpeed = 1

  init(x, y) = function
    this.x = x
    this.y = y
    this.hp = 60
    this.mass = 1
    this.mark("enemy")
    this.collisionShape("circle", 14)
  end

  shootAt(targetX, targetY, shots) = function
    local a = angle(this.x, this.y, targetX, targetY)
    for spread in [-0.25, 0, 0.25] do
      shots.spawn(this.x, this.y, cos(a + spread) * 2.2, sin(a + spread) * 2.2)
    end
  end

  chase(targetX, targetY) = function
    local toTarget = vector(targetX - this.x, targetY - this.y)
    if toTarget.length() > 4 then
      local dir = toTarget.normalize()
      this.x += dir.x * this.moveSpeed
      this.y += dir.y * this.moveSpeed
    end
  end

  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then
      this.defeated.trigger()
      this.vanish()
    end
  end
end
```

```tiberscript
// main.tbs
import "enemies"

states = enum
  intro
  game
  win
  lose
end

damageable = trait
  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then this.died.trigger() end
  end
end

shot = archetype
  init(x, y, vx, vy) = function
    this.x = x
    this.y = y
    this.vx = vx
    this.vy = vy
    this.life = seconds(3)
  end
end

shots = swarm of shot max 1000

shader arena(u, v, t) = function
  local ring = 0.5 + 0.5 * sin(distance(u, v, 0.5, 0.5) * 30 - t)
  return mix(color(0.08, 0.08, 0.12), color(0.18, 0.1, 0.24), ring * 0.6)
end

hero = archetype uses damageable
  signal died()

  init() = function
    this.x = -100
    this.y = 0
    this.hp = 100
    this.mass = 2
    this.friction = 0.1
    this.mark("player")
    this.collisionShape("circle", 14)
  end
end

init() = function
  bind("moveRight", "arrowRight")
  bind("moveLeft", "arrowLeft")
  bind("moveUp", "arrowUp")
  bind("moveDown", "arrowDown")
  bind("attack", "space")
  bind("attack", controllers.button("west"))
  bind("rewind", "r")
  bind("rewind", controllers.button("lt"))

  best = save.get("bossArenaBest", 0)
  time.keep(seconds(5))
  transit(states.intro)
end

entering states.intro do
  shots.clear()
  player = new hero()
  boss = new enemies.goblin(120, 0)
  score = 0
  caption = ""
  winText = register("winText", 0, 10)
  winText.scale = 1

  player.died.connect(function()
    transit(states.lose)
  end)
  boss.defeated.connect(function()
    score = 1000 + player.hp * 10
    transit(states.win)
  end)

  camera.follow("player", 0.1)
  camera.bounds(-300, -200, 300, 200)

  sequence do
    caption = "A boss awaits..."
    wait(seconds(1))
    caption = ""
    transit(states.game)
  end
end

entering states.win do
  if score > best then
    best = score
    save.set("bossArenaBest", best)
    save.write()
  end
  tween("winText", "scale", 2, 30, ease = "bounce")
end

fixedUpdate() = function
  during states.game do
    if action("rewind").held then
      time.rewind(2)
    else
      if action("moveRight").held then player.x += 3 end
      if action("moveLeft").held then player.x -= 3 end
      if action("moveUp").held then player.y += 3 end
      if action("moveDown").held then player.y -= 3 end
      boss.chase(player.x, player.y)
      every(seconds(1.5)) do
        boss.shootAt(player.x, player.y, shots)
      end
      shots.each fast do (s)
        s.x += s.vx
        s.y += s.vy
        s.life -= 1
        if s.life <= 0 then s.despawn() end
      end
      shots.each touching "player" do (s, p)
        p.takeDamage(8)
        s.despawn()
      end

      local hit = collide("enemy")
      if hit then
        resolve(player, hit)
        player.takeDamage(1)
      end
      if action("attack").hit and distance(player.x, player.y, boss.x, boss.y) < 40 then
        boss.takeDamage(10)
      end
    end
  end
end

update() = function
  during states.intro do
    lighting.ambient = 1
    screen.clear("#111")
    screen.text(caption, 0, 0, 20, "white")
  end

  during states.game do
    lighting.ambient = 0.3
    screen.clear("#222")
    screen.shade(arena, 0, 0, screen.width, screen.height, time.now / 20)

    screen.layer(1)
    screen.fillCircle(player.x, player.y, 14, "green")
    screen.fillCircle(boss.x, boss.y, 14, "red")
    shots.draw(circle = 2.5, color = "#FFD750")
    shots.lights(10, "#FFD750", 0.5)
    screen.light(player.x, player.y, 110, "#E0FFE0")
    lighting.draw()

    // the boss's next half second, faintly
    forecast 30 each (future) do
      alter opacity=0.08 do
        screen.fillCircle(future.boss.x, future.boss.y, 14, "red")
      end
    end

    screen.layer(2)
    ui.healthbar(player.hp, 100, -100, 90, 200, 16)
    ui.healthbar(boss.hp, 60, -100, 70, 200, 8)
    if time.rewinding then screen.text("<< REWIND", 0, -80, 14, "#5AA0FF") end
  end

  during states.win do
    lighting.ambient = 1
    screen.clear("black")
    screen.text("Boss defeated!", winText.x, winText.y, 15 * winText.scale, "gold")
    screen.text("Score $(score)   Best $(best)", 0, -30, 14, "white")
    if key("space").hit then transit(states.intro) end
  end

  during states.lose do
    lighting.ambient = 1
    screen.clear("black")
    screen.text("Defeated...", 0, 10, 30, "red")
    screen.text("SPACE to try again, or hold R next time", 0, -30, 12, "white")
    if key("space").hit then transit(states.intro) end
  end
end
```

`shots.clear()` empties the swarm when a new round starts. The arena's floor is the `arena` shader, rings that pulse out from the middle, worked out from `time.now`: hold R and the rings run backward with everything else. A controller plays it too: the left face button attacks and the left trigger rewinds.

## 53. The Whole Game: Tiberian Odyssey 1.2

The 1.0 and 1.1 courses ended with Tiberian Odyssey, the quest for the five crystals of the Crystal of Harmony. Here it is a third time, written the 1.2 way from the first line to the last. Copy it into `main` and press Run.

**Controls:** the arrow keys or a stick walk, SPACE or the bottom face button shoots, SHIFT or the right shoulder button dashes, E or the right face button talks, H or the left face button drinks a potion, S saves, R tries something risky, P or START pauses. On the menu, L loads and O opens the settings screen of the Settings library, once the library is in the project.

What 1.2 brings to it:

- **`const`** for the numbers the game is built on, and **`enum`** for its states: `states.gameOver` is checked as it is written, where a misspelled text would fail with no word.
- **`bind`** and **`action`**: every action has a key and a controller button, and `actionVector()` walks at the speed a stick leans. **`controllers.feel = "fast"`** makes the lightest press count.
- **`match`** picks a sound and the screen to draw, **`"$(...)"`** puts values in texts, and **`value if condition else other`** picks the health line.
- **Defaults and values by name**: `init(name = "Hero")`, `take(collector, loud = true)`, and `start(load = true)`.
- **Signals** on the hero, **`wounded`** and **`found`**, with **inline functions** connected to them, and `array.filter()` with one more inline function to keep only the bullets still flying.
- **Game feel**: `cooldown()` for the shots and the dash, `approach()` for the guards, `springTo()` for the hero's bounce when hurt, `chance()` for a potion a guard may drop, and `seconds()` wherever a time goes.
- **`transition.fade()`** between the menu, the game and the endings, **`colors.mix()`** for a health bar that goes from green to red, **`lighting`** with `screen.light()` and `screen.glow()` for a field at night where the crystals shine, and **`controllers.first.drawButton()`** to show the button the player really has.

The `MUSIC` constant is left empty. Give it the name of a song from your project's Music tab, and the game plays it.

```tiberscript
// Tiberian Odyssey 1.2: a whole game in TiberScript 1.2

const START_HP = 100
const SPEED = 2.4
const ENEMY_SPEED = 0.9
const ENEMY_SIGHT = 100
const ENEMY_DAMAGE = 10
const SAFE_TICKS = 60
const POTION_HEAL = 25
const CRYSTALS_NEEDED = 5
const BULLET_SPEED = 6
const MUSIC = ""

states = enum
  menu
  game
  paused
  gameOver
  win
end

sound(kind) = function
  match kind
    case "shoot" then audio.beep("volume 6 duration 40 square C6 G5")
    case "hit" then audio.beep("volume 10 duration 90 sawtooth C3 G2")
    case "crystal" then audio.beep("volume 8 duration 60 square E5 G5 C6")
    case "defeat" then audio.beep("volume 8 duration 70 noise C4 C3")
    case "heal", "potion" then audio.beep("volume 7 duration 90 sine C5 E5 G5")
    case "dash" then audio.beep("volume 5 duration 50 triangle G4 C5")
    case "save" then audio.beep("volume 6 duration 70 square G5 C6")
    case "win" then audio.beep("volume 9 duration 140 square C5 E5 G5 C6")
    case "lose" then audio.beep("volume 9 duration 180 sawtooth E3 C3 A2")
  end
end

say(text) = function
  dialogue = data(text = text, ticks = seconds(3))
end

bullet = archetype
  init(x, y, dx, dy) = function
    this.x = x
    this.y = y
    this.vx = dx * BULLET_SPEED
    this.vy = dy * BULLET_SPEED
    this.alive = true
    this.mark("bullet")
  end

  update() = function
    this.x += this.vx
    this.y += this.vy
    if abs(this.x) > 260 or abs(this.y) > 160 then
      this.remove()
      return
    end
    local target = collide("enemy")
    if target and target.alive then
      target.defeat()
      this.remove()
    end
  end

  remove() = function
    this.alive = false
    this.vanish()
  end

  draw() = function
    screen.fillCircle(this.x, this.y, 3, "#FFE066")
    screen.glow(this.x, this.y, 10, "#FFE066", 0.6)
  end
end

hero = archetype
  signal wounded(hp)
  signal found(count)

  init(name = "Hero") = function
    this.name = name
    this.hp = START_HP
    this.x = 0
    this.y = 0
    this.w = 16
    this.h = 16
    this.face = vector(1, 0)
    this.size = 1
    this.dashing = 0
    this.score = 0
    this.crystals = 0
    this.potions = 2
    this.safe = 0
    this.mark("player")
  end

  update() = function
    if this.safe > 0 then this.safe -= 1 end
    local steer = actionVector("left", "right", "down", "up")
    if steer.length() > 0.2 then this.face = steer.normalize() end
    local speed = SPEED * 3 if this.dashing > 0 else SPEED
    if this.dashing > 0 then this.dashing -= 1 end
    this.x = clamp(this.x + steer.x * speed, -230, 230)
    this.y = clamp(this.y + steer.y * speed, -140, 140)
    if action("fire").held and cooldown("fire", 12) then
      bullets.push(new bullet(this.x, this.y, this.face.x, this.face.y))
      sound("shoot")
    end
    if action("dash").hit and cooldown("dash", seconds(1)) then
      this.dashing = 8
      this.safe = max(this.safe, 10)
      sound("dash")
    end
    springTo(this, "size", 1)
  end

  hurt(amount) = function
    if this.safe > 0 then return end
    this.hp = max(0, this.hp - amount)
    this.safe = SAFE_TICKS
    this.size = 1.6
    sound("hit")
    shake(4, 12)
    flash("red", 0.35, 10)
    emit("blood", this.x, this.y, 6)
    this.wounded.trigger(this.hp)
  end

  heal() = function
    if this.potions == 0 then
      say("No potions left!")
    elsetry this.hp >= START_HP then
      say("Already at full health")
    else
      this.potions -= 1
      this.hp = min(this.hp + POTION_HEAL, START_HP)
      sound("heal")
      emit("magic", this.x, this.y, 10)
      say("A potion: $(this.hp) HP")
    end
  end

  save() = function
    access "odyssey12_save" as file do
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
    if not exists("odyssey12_save") then
      say("No save yet")
      return
    end
    access "odyssey12_save" as file do
      this.hp = file.hp
      this.x = file.x
      this.y = file.y
      this.score = file.score
      this.potions = file.potions
      for i in file.found do
        crystals[i].take(this, loud = false)
      end
    end
    say("Welcome back, $(this.name)")
  end

  draw() = function
    local blink = this.safe > 0 and Tiber.frame % 10 < 5
    local color = "#FF66FF" if blink else "#66E0FF"
    alter scale=this.size do
      screen.fillRound(this.x, this.y, 16, 16, 0.3, color)
    end
    screen.fillCircle(this.x + this.face.x * 9, this.y + this.face.y * 9, 2, "white")
    screen.light(this.x, this.y, 90, "#FFE0B0")
  end
end

enemy = archetype
  init(id, x, y) = function
    this.id = "Enemy $(id)"
    this.x = x
    this.y = y
    this.w = 16
    this.h = 16
    this.alive = true
    this.mark("enemy")
  end

  update(target) = function
    if not this.alive then return end
    if distance(this.x, this.y, target.x, target.y) < ENEMY_SIGHT then
      this.x = approach(this.x, target.x, ENEMY_SPEED)
      this.y = approach(this.y, target.y, ENEMY_SPEED)
      if collide("player") then target.hurt(ENEMY_DAMAGE) end
    end
  end

  defeat() = function
    this.alive = false
    player.score += 100
    sound("defeat")
    emit("spark", this.x, this.y, 15)
    if chance(0.3) then potions.push(new potion(this.x, this.y)) end
    this.vanish()
  end

  draw() = function
    screen.fillRect(this.x, this.y, 16, 16, "#FF5050")
    alter opacity=0.1 do
      screen.fillCircle(this.x, this.y, ENEMY_SIGHT, "#FF5050")
    end
  end
end

potion = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.taken = false
    this.mark("potion")
  end

  take(collector) = function
    if this.taken then return end
    this.taken = true
    collector.potions += 1
    sound("potion")
    say("A potion dropped: $(collector.potions) now")
    this.vanish()
  end

  draw() = function
    screen.fillRound(this.x, this.y, 8, 10, 0.5, "#78DC8C")
    screen.glow(this.x, this.y, 16, "#78DC8C", 0.7)
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

  take(collector, loud = true) = function
    if this.taken then return end
    this.taken = true
    collector.crystals += 1
    if loud then
      collector.score += 150
      sound("crystal")
      emit("coin", this.x, this.y, 8)
      collector.found.trigger(collector.crystals)
    end
    this.vanish()
  end

  draw() = function
    local y = this.y + wave(Tiber.frame + this.index * 15, 3)
    alter rotation=Tiber.frame * 2 do
      screen.fillTriangle(this.x - 6, y - 5, this.x + 6, y - 5, this.x, y + 8, "#FFD750")
    end
    screen.glow(this.x, y, 22, "#FFD750", 0.9)
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
    say("$(this.name): Hello, traveler!")
  end

  draw() = function
    screen.fillRound(this.x, this.y, 16, 18, 0.4, "#AAAAFF")
    screen.text(this.name, this.x, this.y + 16, 9, "#AAAAFF")
    screen.light(this.x, this.y, 50, "#AAAAFF", 0.6)
  end
end

elder = archetype extends character
  init() = function
    super.init("Elder Tim", 0, 65)
    this.asked = false
  end

  talk() = function
    if not this.asked then
      this.asked = true
      say("Elder Tim: Collect $(CRYSTALS_NEEDED) shards to save us!")
      schedule(seconds(10)) = function
        if npcs[0].asked and game_state == states.game then say("Elder Tim: Keep searching, they glow in the dark!") end
      end
    elsetry player.crystals >= CRYSTALS_NEEDED then
      say("Elder Tim: You restored the Harmony!")
      win()
    else
      say("Elder Tim: Need $(CRYSTALS_NEEDED - player.crystals) more crystals!")
    end
  end
end

player = null
enemies = []
crystals = []
potions = []
npcs = []
bullets = []
dialogue = data(text = "", ticks = 0)
game_state = states.menu
nearby = null

found_list() = function
  local found = []
  for c in crystals do
    if c.taken then found.push(c.index) end
  end
  return found
end

clear_world() = function
  for group in [enemies, crystals, potions, npcs, bullets] do
    for thing in group do
      if not thing.taken and thing.alive != false then thing.vanish() end
    end
  end
  if player then player.vanish() end
end

new_world() = function
  clear_world()
  player = new hero()
  player.wounded.connect(function(hp)
    if hp <= 0 then lose() end
  end)
  player.found.connect(function(count)
    if count == CRYSTALS_NEEDED then say("All $(count) crystals: back to the Elder!") end
  end)
  enemies = [new enemy(1, -200, 120), new enemy(2, 200, 120), new enemy(3, -200, -120), new enemy(4, 200, -120)]
  crystals = [new crystal(0, -100, -50), new crystal(1, 100, -50), new crystal(2, -100, 50), new crystal(3, 100, 50), new crystal(4, 0, -20)]
  potions = []
  npcs = [new elder()]
  bullets = []
  dialogue = data(text = "", ticks = 0)
  nearby = null
end

go(state) = function
  game_state = state
  transit(state)
end

win() = function
  silence("all")
  sound("win")
  go(states.win)
end

lose() = function
  silence("all")
  sound("lose")
  go(states.gameOver)
end

start(load = false) = function
  if transition.active then return end
  transition.fade(0.4) do
    if load then player.load() end
    tune(MUSIC)
    go(states.game)
  end
end

init() = function
  controllers.feel = "fast"
  bind("left", "arrowLeft")
  bind("left", controllers.button("left"))
  bind("right", "arrowRight")
  bind("right", controllers.button("right"))
  bind("up", "arrowUp")
  bind("up", controllers.button("up"))
  bind("down", "arrowDown")
  bind("down", controllers.button("down"))
  bind("fire", "space")
  bind("fire", controllers.button("south"))
  bind("dash", "shift")
  bind("dash", controllers.button("rb"))
  bind("talk", "e")
  bind("talk", controllers.button("east"))
  bind("heal", "h")
  bind("heal", controllers.button("west"))
  bind("pause", "p")
  bind("pause", controllers.button("start"))
  new_world()
  print("Welcome to Tiberian Odyssey 1.2")
  go(states.menu)
end

update() = function
  during states.menu do
    if action("fire").hit then start() end
    if key("l").hit then start(load = true) end
    if key("o").hit then Tiber.settings.open() end
  end

  during states.game do
    player.update()

    for b in bullets do
      if b.alive then b.update() end
    end
    bullets = array.filter(bullets, function(b)
      return b.alive
    end)

    for e in enemies do
      e.update(player)
    end
    if game_state != states.game then return end

    local gem = collide("crystal")
    if gem then gem.take(player) end
    local drop = collide("potion")
    if drop then drop.take(player) end

    nearby = near("npc", 40)
    if nearby and action("talk").hit then nearby.talk() end
    if game_state != states.game then return end

    if action("heal").hit then player.heal() end
    if key("s").hit then player.save() end

    if key("r").hit then
      attempt
        if chance(1 / 3) then chest.lock.open() end
        player.score += 50
        say("A risk that paid: +50")
      recover
        say("The risk went wrong")
        player.hurt(20)
      end
    end

    every(seconds(5)) do
      emit("magic", random(-140, 140), random(-80, 80), 2)
    end

    if dialogue.ticks > 0 then dialogue.ticks -= 1 end

    if action("pause").hit then
      pause("music")
      go(states.paused)
      return
    end
  end

  during states.paused do
    if action("pause").hit then
      resume("music")
      go(states.game)
      return
    end
  end

  during states.gameOver do
    if action("fire").hit then back_to_menu() end
  end

  during states.win do
    if action("fire").hit then back_to_menu() end
  end
end

back_to_menu() = function
  if transition.active then return end
  transition.fade(0.4) do
    new_world()
    go(states.menu)
  end
end

draw() = function
  lighting.ambient = 0.3 if game_state == states.game else 1
  match game_state
    case states.menu
      screen.clear("#000033")
      screen.text("Tiberian Odyssey 1.2", 0, 45, 22, "#FFD750")
      screen.text("to start, L to load, O for settings", 18, 5, 11, "white")
      local pad = controllers.first
      if pad then
        pad.drawButton("south", -110, 5, 9)
      else
        screen.text("SPACE", -112, 5, 11, "#FFD750")
      end
      screen.text("arrows or a stick move, SHIFT dashes, E talks", 0, -25, 9, "#8888AA")
      screen.text("H heals, S saves, R risks, P pauses", 0, -40, 9, "#8888AA")
    case states.paused
      screen.clear("#111111")
      screen.text("PAUSED", 0, 15, 28, "white")
      screen.text("P to go on", 0, -15, 14, "#AAAAAA")
    case states.gameOver
      screen.clear("black")
      screen.text("GAME OVER", 0, 25, 30, "#FF5050")
      screen.text("Score $(player.score)", 0, -5, 16, "white")
      screen.text("SPACE to try again", 0, -35, 13, "#AAAAAA")
    case states.win
      screen.clear("#002200")
      screen.text("VICTORY!", 0, 30, 32, "#FFD750")
      screen.text("The Crystal of Harmony is whole", 0, 0, 14, "white")
      screen.text("Final score $(player.score)", 0, -22, 15, "#AAFFAA")
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
    for p in potions do
      if not p.taken then p.draw() end
    end
    for n in npcs do
      n.draw()
    end
    for e in enemies do
      if e.alive then e.draw() end
    end
    for b in bullets do
      b.draw()
    end
    player.draw()
    if nearby then screen.text("E to talk", nearby.x, nearby.y - 18, 9, "white") end
  end

  local health = player.hp / START_HP
  screen.fillRect(-130, 88, 80, 6, "#333344")
  screen.fillRect(-170 + 40 * health, 88, 80 * health, 6, colors.mix("#FF5050", "#78DC8C", health))
  screen.text("Crystals $(player.crystals)/$(CRYSTALS_NEEDED)", 0, 88, 12, "white")
  screen.text("Score $(player.score)  Potions $(player.potions)", 130, 88, 11, "#FFD750")

  local status = "Healthy" if health > 0.6 else "Injured" if health > 0.3 else "CRITICAL"
  screen.text(status, -130, 76, 10, colors.mix("#FF5050", "#78DC8C", health))

  if dialogue.ticks > 0 then
    alter opacity=0.85 do
      screen.fillRound(0, -55, 300, 26, 0.3, "#111827")
    end
    screen.text(dialogue.text, 0, -55, 11, "#FFD750")
  end

  screen.text("SPACE shoot  SHIFT dash  E talk  H heal  S save  R risk  P pause", 0, -88, 8, "#555577")
end
```

**Try it.** Turn `lighting.ambient` down to 0.1 and play in the dark. Then give `time.keep()` a try from Lesson 33, and a key that rewinds the last two seconds after a bad hit.

## 54. Graduation

### You are a TiberScript 1.2 Developer!

You now know the full game script upgrade on top of everything 1.1 already gave you, and the one thing no other game language has.

**What is new since 1.1:**

* `time.keep()`, `time.rewind()`, `time.forecast()`, `forecast ... each`, `whatif`, `time.replay()`, `timeless`, `network.rollback()`, `online`: time as a value, and a game online in one word
* `html name ... end`, `$( )`, `do=`, `bind=`, `show=`, `each=`: pages written in the game's own file, kept up to date by the Forge
* `retro`, `time.changes()`, `time.undoChange()`, `time.showOldPast()`, `time.changeThePast`: changing the past, and the present it makes
* `swarm of`, `.spawn()`, `.each`, `.each touching`, `.despawn()`, `.draw()`, and `where`: a million things at once
* the Forge: compiled, changed while it runs, and JavaScript native to it: `javascript ... end` blocks in full JavaScript, `javascript` functions and methods, `js`, promises and callbacks
* `fast` and `.each fast`: WebAssembly speed, and the same answer on every machine; an `each` of numbers alone runs as WebAssembly by itself
* `capture.draw()`, `capture.frameTick()`, `item.info`: a game's own gallery of its best moments, a clip stepped one picture at a time
* `buffer()`, `wasm`, `Tiber.forge`: numbers every language shares, WebAssembly of your own, and the say over the Forge
* `tiberscript 1.1 ... end`, `tiberscript 1.0 ... end`, `Tiber.version = "1.1"`: old code kept as it is, inside a 1.2 game
* `colors.mix()`, `colors.lighten()`, `colors.darken()`, `colors.fade()`, `colors.hsl()`: colors made from colors
* `transition.fade()`, `transition.wipe()`, `transition.iris()`, `transition.blinds()`: the moment between two screens
* `springTo()`: a number that arrives with a bounce
* `watch()`: your game's values on its own screen
* `Tiber.motion`, `Tiber.prefersLessMotion`, `Tiber.translate`, `Tiber.frameRate = "auto"`, `Tiber.renderScale = "auto"`: a game for every player and every computer
* controllers read the moment they report, with `pad.pace`, `pad.motion`, `pad.battery` and `pad.setLight()`
* `pad.repeat()`, `pad.tapped()`, `pad.longPress()`, `pad.doubleTap()`, `pad.heldFor()`, `key().repeat`, `action().repeat`, `repeats()`: buttons that know how long they are held
* `controllers.threshold`, `pad.setThreshold()`, `controllers.curve`, `controllers.amount()`, `pad.raw()`, `"rsUp"`: how far down counts, and sticks as fine as you want them
* `controllers.feel`, `"auto"`, `"light"`, `"wide"`, `"aim"`, `pad.drift`: controller feel in words, or measured from the controller in the player's hands
* `"touchpad"`, `"mute"`, `"chat"`, `"paddle1"`, `"cUp"`, `pad.touches`, `pad.has()`, `pad.label()`, `pad.setTrigger()`: every button a controller has, by one name on every controller
* `key("primary")`, `key("command")`, Control Shift and a number for an F key, `system.platform`: one keyboard for every computer
* `mouse.wheel`, `mouse.released`, and a click that is never missed: the mouse
* the Settings and PerfMonitor libraries, `Tiber.settings` and `Tiber.hold()`: a settings screen with a Controller Check that is also a pause that really pauses, the frame rate the screen really shows, and screens of your own over the game
* defaults, values by name, `match`, `const`, `if ... else` values, `otherwise`, `in`, two names in a loop, `wait until`: the language, rounded out
* `approach()`, `remap()`, `wrap()`, `ease()`, `cooldown()`, `chance()`, `weightedChoice()`, `grid()`, `type()`, `inspect()`, `tr()`: game feel, built in
* `.amount`, `actionVector()`, `.within()`, `.consume()`, `.heldFor`, `.releasedWithin()`: analog input, and input that forgives
* `shader`, `glsl`, `screen.shade()`, `screen.shader()`, `pixel()`, `previous()`, `image()`, `fbm()`, `voronoi()`, `blend =`, `quality =`, lists and buffers as parameters, `sdCircle()`, `sdBox()`, `.drawPoints()`, `.moveOnChip()`: your own code on the graphics chip, for every pixel and every member of a swarm
* `effects.filter()`: your own shaders stacked on the finished frame
* `physics.add()`, `physics.push()`, `.grounded`, `.contacts`: bodies that fall, stack and bounce, and rewind exactly
* `lighting`, `screen.light()`, `screen.spotlight()`, `screen.sunlight()`, `screen.glow()`, `lighting.block()`, `lighting.blockCircle()`, `swarm.lights()`, `lighting.at()`, `lighting.colorAt()`, `effects`: light, shadow, and a finish on the frame
* `lighting.mirror()`, `screen.mirror()`, `screen.water()`: mirrors that throw light back, and reflections of the picture
* `screen.lens()`, `screen.shockwave()`: the picture bent round a point, quick in every browser
* `screen.compute()`: a shader worked out on the graphics chip, its numbers handed back to the game
* `time.stopwatch`: milliseconds with their fractions, for timing your own work
* `time.rewind(0.5)`, `time.forward()`, `time.ago()`, `time.speed`, `time.slow()`, `time.freeze()`: time at any speed, forward again, and a picture of the past
* `Tiber.forge.profile`, `Tiber.forge.hot()`, `Tiber.forge.cache`, `wasm.memory`: where the time goes, the code the browser keeps, and one memory for every language
* `controllers.direct`, `controllers.connect()`, `pad.motion`, `pad.battery`: Nintendo's controllers read from their own reports
* `platform.unlock()`, `.setStat()`, `.submitScore()`, `.cloudSave()`: achievements, stats, leaderboards and cloud saves, on the web and on Steam
* `Tiber.tickRate = "auto"`, `Tiber.tickScale`: a game that runs 120 or 240 updates a second by itself on a fast screen
* `Tiber.resolution`, `Tiber.shaderScale`: 720p for an old laptop, 4K for a new one, from one line
* `tilemap()`, `noise()`, `path.find()`: solid maps, smooth randomness, and the way around
* `import`, `export`: real multiple file projects, with exported values shared between them
* `Tiber.create()`, `Tiber.libraries`: your own libraries, in every project you make
* `elseif`: `elsetry` written another way
* the checks a game gets as it starts in the editor: misspelled names, missing functions, sprites and sounds, said with their line
* `"$(value)"`, `seconds()`, `function(...) ... end`: interpolation, time in seconds, inline functions
* loop counters that belong to their loop
* `string`, `array`, `vector`, `math`, `table`: a real standard library
* `trait`, `uses ... and ...`: shared behavior without forced inheritance
* `export` on a value: tunable from outside code
* `enum`: typo checkable named states
* `entering`, `leaving`: run once when a state starts or ends
* `sequence`, `wait`: pausable cutscenes and timed sequences
* `on`, `trigger`, `signal`, `.connect()`: two ways for objects to talk to each other
* `gamepad()`, `controllers`, `.drawButton()`, `.rumble()`, `controllers.setup()`, `controllers.report()`: every controller, wired or wireless, by where its buttons are
* `bind()`, `action()`, `touch.button()`: input mapped to intent
* `.animate()`, `.playAnimation()`: sprite animation
* `camera.follow()`, `.bounds()`, `.layer()`, `screen.layer()`: a real camera and draw order
* `tween(..., ease = ...)`: nonlinear motion
* `audio.setVolume()`, `audio.has()`: real channels
* `ui.button()`, `ui.healthbar()`, `ui.textbox()`, `dialogue`, `choice`: built in UI and conversations
* `array.ofObjects()`: tagged objects as a real list
* `fixedUpdate()`, `.mass`, `.friction`: steady physics
* `.collisionShape()`, `.collisionLayer()`, `.collidesWith()`, `resolve()`: real collision
* `test`, `assert`: on demand correctness checks
* `Tiber.extendArchetype()`, `Tiber.hasFeature()`: a bigger Tiber
* `save.set()`, `save.get()`, `save.setObject()`, `save.getObject()`, `save.write()`, `save.list()`, `save.delete()`: structured saves
* `network.send()`, `network.on()`: multiplayer hooks
* `schedule()` handles and `cancelSchedule()`, plus `seedRandom()`, `randomFloat()`, `randomBool()`, `randomChoice()`, `randomSign()`
* `wait(signal)`, `Tiber.showDebugOverlay()`: waiting on a signal instead of a frame count, and a live debug panel

Everything from 1.1 still works. Go build the full game, give it a rewind button, change its past, put it online in one word, fill the screen with bullets, paint its sky with a shader, stack crates that fall when you rewind them, light it with a torch, give it achievements, and hand a friend a controller.
