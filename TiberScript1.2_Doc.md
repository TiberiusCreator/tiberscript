# The Official TiberScript 1.2 Handbook

**Version:** 1.2

**Created by:** TiberiusCreator

**Inspired by:** microScript, GDScript and Python

**Special Thanks to:** gilles who created microStudio and created microScript!

> **This is the 1.2 Handbook, checked on 10/28/2026.** It is the whole of TiberScript 1.2, exactly as microStudio runs it for every project set to TiberScript. Every code block in it is run on the engine by microStudio's own tests. It builds on the 1.1 Handbook, and the 1.2 Tutorial teaches the same things lesson by lesson. The 1.0 and 1.1 Handbooks stay true for what they cover: a program written for either runs here as it is.

---

## What's New in 1.2

1.1 made TiberScript real. 1.2 makes it a full game scripting language. Every system a full length game actually needs is here now, and nothing from 1.1 goes away.

And then there is the one thing no other game language has.

- **THE BIGGEST UPDATE OF ALL, in six parts.** See Section 19.
  - **Time Is a Value.** Your game can rewind itself, as far back as the whole game if you want, look into its own future, try "what if" without it happening, replay any moment, and play online with rollback netcode, all as plain TiberScript. And `online`, one word, puts a game online: a room, a lobby with a link and a QR code to join from another window or a phone, players who watch, leave and come back, and rollback underneath.
  - **A Million Things at Once.** `swarm of bullet` keeps a crowd as a few blocks of numbers: a quarter of the memory, no garbage collection stutter, and snapshots the Timeline takes in a third of a millisecond.
  - **The Forge: JavaScript and WebAssembly.** TiberScript compiled to JavaScript, the browser's own fastest language: 67 to 492 times faster than 1.1 on its benchmarks, 18 times faster on a real game's frame in its first minute and 25 times after it, and 345 times faster on a heavy one, with a game changed while it runs without starting over. JavaScript itself is written right inside TiberScript: `javascript ... end` opens a block that is JavaScript in full, colored as JavaScript in the editor, whose functions, classes and values are the game's own names; a function or a method can be in JavaScript, every name of the page is there through `js`, a sequence can wait for a promise, and callbacks go both ways. One word, `fast`, has the Forge compile a function to WebAssembly instead: a crowd moves in about a fifth of the time, and the math gives the same answer on every machine. And there is more to WebAssembly than the word: a `buffer` is a block of numbers that TiberScript, JavaScript, WebAssembly and shaders all read in the same place with nothing copied, `wasm` reads the module the Forge wrote and runs one of your own, and `Tiber.forge` says what the Forge made of every function, shows the code it wrote, times it, and lets a game choose.
  - **The Graphics Chip.** Put `shader` in front of a function and the Forge writes it for the graphics chip, which runs it for every pixel at once: skies, water, fire, clouds, heat haze, shockwaves and black holes that bend the picture, all in TiberScript, about a millisecond for a whole 1080p frame. A shader reads sprites, the frame it drew last time (trails, smoke, ripples), layered noise and scattered cells, lists of numbers and buffers, draws shapes by their distance, draws with eight blend modes and an opacity, and can be written in GLSL itself. Each draw has a `quality`, `Tiber.shaderScale` sets it for every shader at once, and `Tiber.resolution` draws the whole game at 720, 1080 or 1440 lines, or in 4K. The same shader runs on the processor in a test or on a computer without a graphics chip, with the same colors, and `swarm.drawPoints()` draws a million things in 5 ms, 28 times faster than before.
  - **HTML, Powered by the Whole Forge.** `html shop ... end` is a page written in the game's own file: HTML, CSS and JavaScript as the web writes them, with `$(gold)` for a value the page always shows as it is now, `do="buy(item)"` for the game's own code on a click, `bind="volume"` for an input kept equal to a value both ways, `show=` for a part shown only while it is true, and `each="item in bag"` for one part for every item of a list. The Forge compiles all of it with the game, and only what changed reaches the page.
  - **Change the Past.** The biggest of the six. `retro seconds(2) do ... end` makes a change two seconds ago and plays the game forward again from there with the same inputs, so the present becomes the one that change would have made. Save your code while the game runs, or move a tunable's slider, and all the time kept is played again with it: the run you are in becomes the run where the bug never happened. `time.undoChange()` puts back what had happened, and `time.showOldPast(true)` draws it faintly over the new present.
- **Light and effects.** Lights, spotlights, sunlight and real shadows, soft or sharp, from walls, tile maps, round things, objects and whole swarms, a glow around every light, glows that light nothing, a light on every member of a swarm, and a finish on the whole frame (bloom, scanlines, a vignette, color fringes, big pixels, film grain, or an old television, all at once). A light is drawn like everything else, so a rewound, replayed or forecast game is lit right with no work at all. See Section 23.
- **Mirrors and water.** `lighting.mirror()` is a mirror that light does not pass and that throws every light back, shadows and all, so a lantern in front of one lights the room twice and a spotlight comes back as a beam. `screen.mirror()` is a rectangle that shows the picture beside it turned over, and `screen.water()` is the same with ripples, a tint and a fade: a lake, a wet floor, a pane of glass, each in one line. See Section 23.13.
- **Physics that rewinds.** `physics.add(crate, "box", 16, 16)` and a crate falls, stacks, slides and is pushed; balls bounce and roll. It keeps nothing of its own, so a rewind, a forecast, a replay and a rollback move every body exactly, and every machine gets the same numbers. See Section 24.
- **Filters, and swarms the graphics chip moves.** `effects.filter(shader)` stacks your own shaders on the finished frame, in order, each with its strength, and `swarm.moveOnChip(shader)` has the graphics chip move every member of a swarm at once. See 23.11 and Section 25.
- **A whole game saved in one line.** `save.game()` writes down everything the game is right now, and `save.loadGame()` puts all of it back. See Section 15.
- **Pictures and clips of the best moments.** `capture.moment("the boss falls")` keeps a picture of the frame, and a clip of the seconds around it; with `capture.auto` on, the engine keeps its own: a sprite seen for the first time, a sudden burst of action, an action the game names. Off unless a game or its player asks. See Section 26.
- **Time at any speed.** `time.rewind(0.5)` goes back at half speed and `time.rewind(0.25)` at a quarter, `time.forward()` goes forward again over what was just gone back over, `time.ago()` is a picture of the world a while ago, and `time.speed` slows the whole game down or hurries it up, drawn smoothly in between: slow motion with `time.slow()`, a hit that stops the world for a moment with `time.freeze()`, one tick at a time with `time.step()`. See Section 19, Part One.
- **Controllers that answer as they do on their own console.** Xbox, PlayStation (the DualSense and DualSense Edge too), Nintendo Switch (the Pro Controller and Joy Con, two Joy Cons as one controller), Steam, 8BitDo, Logitech, PowerA, HORI, GameSir and any other controller, plugged in or over Bluetooth. Where microStudio runs, Nintendo's, Sony's and Microsoft's controllers are read from their own reports the moment each one arrives, at the controller's own pace and not at the pace a browser passes changes on, so a press reaches the game with no wait added. A light touch answers, the shortest tap is never missed, each stick uses the calibration its own controller carries, and the motion sensor (`pad.motion`), the battery (`pad.battery`), rumble on the controller's own motors and the light that shows which player it is all work. Every player keeps their number when a controller is unplugged, every brand's button names work, button prompts are drawn to match the controller in the player's hands, `pad.pace` says how often a controller is read, and a setup screen that any game opens with F2 teaches the engine any controller a browser does not know. See Section 7.
- **From one screen to the next.** `transition.fade(0.4) do ... end` covers the screen, runs the block while nobody can see, and uncovers it: a title that becomes a level, a door, a death and a new try. `transition.wipe()`, `transition.iris()` and `transition.blinds()` do the same with a curtain, a closing circle and strips. See Section 5.
- **Colors that can be worked with.** `colors.mix()`, `colors.lighten()`, `colors.darken()`, `colors.fade()`, `colors.rgb()` and `colors.hsl()` make a color from another: a health bar that goes from red to green, a button a shade lighter under the pointer, a whole palette from one color. See Section 2.
- **Springs.** `springTo(button, "size", 1)` takes a number to its target with a bounce, the way a pressed button, a landing hero or a panel sliding in should move. See Section 2.
- **Values on the screen while you work.** `watch("speed", hero.vx)` shows a value in a corner of the running game, every tick, with no console to scroll. See Section 12.
- **Buttons that know how long they are held.** `pad.repeat("down")` keeps a menu moving while a direction is held, `pad.tapped()`, `pad.longPress()` and `pad.doubleTap()` tell a tap from a hold, and `pad.heldFor()` counts the ticks. A key and an action answer the same way (`key("arrowDown").repeat`, `action("down").repeat`), and so do the engine's own menus. See Section 7.
- **How far down counts, set by the game or by the player.** `controllers.threshold` is how far a button or a trigger goes down before it counts, `pad.setThreshold("rt", 0.8)` sets one button alone, `controllers.curve` makes the middle of a stick finer, `pad.raw("leftX")` reads a stick as it is, and each stick is four directions of its own (`"rsUp"`, `"lsLeft"`). Each setting takes a word (`controllers.threshold = "light"`, `controllers.deadzone = "wide"`), `"auto"` lets the engine measure the controller in the player's hands, `controllers.feel = "fast"` (or `"normal"`, `"precise"`, `"relaxed"`) sets them all at once, and `pad.drift` shows what was measured. See Section 7.
- **Every button a controller has.** Past the face buttons, the shoulders, the triggers and the sticks: the extra front button each maker puts somewhere (a touchpad pressed in, Share, Capture, Star, a quick access button), a finger resting on a touchpad and where it is, a microphone button, a chat button, up to four back buttons, the small rail buttons of a single Joy Con, and the C buttons of a controller made for the Nintendo 64, read as a right stick. Each has one name a game uses on every controller, and the names each maker prints answer too. `pad.has("paddle1")` says whether the controller in the player's hands has that button, and `pad.label()` says what it is called there. See Section 7.
- **A settings screen in every game.** F4, or START and SELECT held together, opens one screen the engine draws: pictures a second, picture size, detail, shaders, shake and flash, rumble, how hard a button is pressed, where a stick rests and how it feels, and controller setup. The game waits behind it, what the player chose is kept, and a game's own menu reaches the same lines with `Tiber.settings`. See Section 21.
- **Less motion for a player who asks.** `Tiber.motion` scales every shake and every flash, down to none, and `Tiber.prefersLessMotion` says what the player's own system asks for. See Section 13.
- **TiberScript 1.0 and 1.1, inside 1.2.** `tiberscript 1.1 ... end` reads what is inside it by the 1.1 rules, `tiberscript 1.0 ... end` by the 1.0 rules, and `Tiber.version = "1.1"` on a line of its own does the same for a whole file. Old code is kept exactly as it was written, next to new code, in one game. See Section 1.
- **More say over the Forge.** `Tiber.forge.profile` counts and times every call and `Tiber.forge.hot()` names the functions that took the most, `Tiber.forge.cache` and `Tiber.forge.clearCache()` rule the compiled code the browser keeps, and `wasm.memory` hands the game's own fast memory to a WebAssembly module made elsewhere, so it works on the game's buffers where they are. See Section 19, Part Three.
- **60, 120 or 240 frames a second, and up to 240 updates, on their own.** `Tiber.frameRate` draws in between updates on a fast screen, `Tiber.renderScale` keeps older computers at full speed, the performance monitor (F3) has a switch for pictures a second and one for updates a second (GAME, 60, 120, 240 or AUTO), a game written with `Tiber.tickRate = "auto"` runs 120 real updates a second by itself on a 120 Hz screen with room to spare, and 240 on a 240 Hz one, and the updates are locked to the screen's refresh so motion never stutters. See Section 21.
- **HTML pages over the game.** `html.show("shop")` puts one of the project's own web pages over the game, `html.receive()` reads what the player chose there and `html.set()` fills it in, the same in every language, and `html.page()` makes a page from a text, so a `javascript` block can hold a whole page, HTML, CSS and its own script. See Section 9.
- **Built in platform SDKs.** `platform.unlock()`, stats, leaderboards, cloud saves, the player's name and presence: kept on the player's computer in a browser, sent to Steam or Epic Online Services in a desktop export, and ready for a console's own SDK. See Section 22.
- **A game in another language, in one line.** `Tiber.translate = "es"`, or `Tiber.translate("japanese")`, draws every text of the game in that language, in the same place, color and width, with its numbers kept as numbers. The words come from the game's own translations, from the ones the editor kept in the project, or from the browser's own translator where it has one. See Section 2.
- **Your own libraries, in every project.** `Tiber.create("tools")` in a file keeps that file as a library on your computer, and every one of your TiberScript projects reaches what it exports as `Tiber.tools`. `Tiber.libraries` lists them, and the editor's library panel shows them. See Section 3.
- **Mistakes found as the game loads.** When a game starts in the editor, all of its files are read together for the mistakes that would otherwise wait for their line to run, or never say anything: a misspelled name and the one it was meant to be, a function nobody made, a function given more values than it takes, a function that has a built in's name, a misspelled built in, a sprite, sound or song the project does not have. And a message from the running game names what was null and what was meant. See Section 12.
- **`elseif`.** Another way to write `elsetry`: either one is fine, the way `archetype` and `class` are. See Section 1.
- **Every name colored.** The editor colors every built in, every object of the engine and every keyword of 1.2 as you type. See Section 18.
- **The language, rounded out.** Parameters with values of their own (`spawn(x, y, speed = 1)`), values given by name in any order (`spawn(y = 9, x = 4)`), `match` with several values and ranges, `const`, a value chosen by a condition (`"alive" if hp > 0 else "down"`), `otherwise` for a value that may be null, `in` and `not in`, a loop over two names (`for name, value in stats`), and `wait until` in a sequence. See Sections 1 and 5.
- **Game feel, built in.** `approach()`, `remap()`, `wrap()`, `round(n, digits)`, `ease()` with eleven curves, `chance()`, `weightedChoice()`, `cooldown()` that rewinds with the world, `grid()`, `type()`, `inspect()`, `array.shuffle()`, `.sum()`, `.average()`, `.unique()`, `.first()` and `.last()`, and `tr()` for a game in more than one language. See Section 2.
- **Input that forgives.** A jump pressed a moment before landing still jumps (`action("jump").within(8)`), `heldFor` and `releasedWithin()` for charged attacks and short hops, and `amount`, `actionAxis()` and `actionVector()` for a stick that steers round a smooth circle at every speed in between. See Section 7.
- **Modules.** `import` and `export`. A real project can finally live in more than one file, a library is one file to import, and a value a module exports is shared, not copied.
- **The Standard Library.** Real string, array, math, and table helpers instead of leaning on built in functions alone.
- **Inline functions.** `function(a, b) ... end` as a value, for callbacks you only need once.
- **Sequences.** `sequence do ... wait(N) ... end` for cutscenes, dialogue, and timed attack patterns.
- **Events.** A quick global `on()` and `trigger()` for simple cases, and real declared `signal` blocks for anything bigger.
- **Enums.** Named states instead of bare strings, so a typo is a real error instead of a silent blank screen.
- **State hooks.** `entering` and `leaving` blocks run once when a state starts and ends.
- **Traits.** `uses` lets an archetype share behavior with as many traits as it needs.
- **Tunable variables.** `export` a value and change it without touching code.
- **Dialogue.** A real branching conversation system, built in.
- **Action mapping.** `bind()` and `action()`. Input tied to intent, not physical keys.
- **Sprite animation.** Frame sequences and animation state, built in, and any one frame of a sprite drawn by name, mirrored when it faces the other way.
- **Camera upgrades.** Smooth follow, bounds, and parallax layers.
- **Draw layers.** Real control over what renders in front of what.
- **Real collision.** Shapes beyond boxes, actual resolution, and layers and masks.
- **Fixed timestep physics.** A `fixedUpdate()` that runs at a steady rate no matter the frame rate.
- **UI primitives.** Buttons, health bars, and word wrapped text boxes.
- **Structured save data.** A real save system, not just a slot that exists or does not.
- **Networking hooks.** microStudio's multiplayer, finally reachable from TiberScript.
- **Upgraded physics.** Mass and friction, not just gravity and bounce.
- **Tween easing.** Bounce, elastic, and ease curves, not just linear motion.
- **Cancellable timers.** A schedule you can call off before it fires.
- **Time in seconds.** `seconds(1.5)` anywhere a frame count goes.
- **`where`.** `for e in enemies where e.hp < 10 do` picks out part of any list or swarm, in the loop itself.
- **A bigger randomness toolkit.** Seeding, decimals, coin flips, picking from a list, and random direction.
- **Audio channels.** Separate music and sound effect volume, and `audio.has()` to ask what a project contains.
- **String interpolation.** `"Score: $(score)"` without a single `+`.
- **Testing.** `test` and `assert` so you can trust your own code as your game grows.
- **Better errors.** Messages that say what was expected, not just what broke.
- **Typed built ins.** Every built in function now has a known signature, the foundation real autocomplete needs.
- **A bigger Tiber namespace.** Version checks and feature checks, alongside everything it already did.
- **Signal aware waiting.** `wait()` now pauses for a signal, not just a frame count.
- **Whole object saves.** Save and restore an entire archetype instance in one call.
- **A live debug overlay.** See every trigger fire, every tag on the object under your cursor and where each frame's time went, and scrub back through time.
- **Tile maps with collision.** Stand on tiles, stop at walls, jump up through ledges, in one call.
- **Noise and pathfinding.** Smooth randomness for worlds, and a shortest route around them.

All 1.1 scripts run unchanged, with one deliberate exception, explained in Section 1: a `for` loop's counter now belongs to its loop. Nothing above replaces anything. It all sits on top.

1.2 is what microStudio runs for every project set to TiberScript. The language menu offers **TiberScript 1.2**, and that is what a new project gets. TiberScript 1.1 is still inside microStudio, frozen exactly as it shipped, but it is no longer offered in the menu: a project that was already set to 1.1 keeps running on it and still shows its name, and a new project never needs it, because every 1.1 program runs on 1.2. Every 1.1 program has been played on both engines, frame by frame, with the same keys and the same random numbers, and draws exactly the same on each. Every code block in this handbook is run on the engine as a test, and every part of 1.2 has tests of its own.

1.1 itself grew during the 1.2 work, and those additions are part of 1.1, not 1.2. The final 1.1 check added `0` counting as false, `alter color=` and `alter rotation=`, `view world`, `Tiber.state` and `Tiber.frame`, `min()` and `max()` of any number of values, functions called from fields, and round circles. Before that came `%` and `%=`, string escapes, single quoted strings, `for ... in`, the string methods `.endsWith()`, `.contains()`, `.toUpper()`, `.toLower()`, `.trim()`, `.substring()`, `.replace()`, `.split()`, the list methods `.insert()`, `.pop()`, `.contains()`, `.indexOf()`, `.join()`, `.sort()`, `.reverse()`, `alter volume= pitch=`, a working `pause("music")` and `resume("music")`, `Tiber.version` and `Tiber.lastError`, the rest of microStudio's screen, touch, gamepad, system and storage, and `emit()`, `flash()` and `shake()` that show in a game with its own `draw()`. The 1.1 Handbook covers all of them.

---

#### TiberScript 1.2 is the same language you already know. No semicolons. No curly braces. No colons. Scoped logic that cleans up after itself. Code that reads like a sentence. 1.2 is about scale. It gives that same language everything a full length game needs to actually get finished, checked against how other established game scripting languages solve the same problems, not built in isolation.

A note on naming before anything else: every new built in below is lowercase, the same as `screen`, `mouse`, and `key` already were in 1.1. `Tiber` is the one deliberate exception, since it is not a generic utility namespace. It is the language's own name.

---

## 1. The Core Syntax

Comments, function definitions, `local`, `return`, operators (`%` included), booleans, `null`, loops, string escapes, and `data` blocks all work exactly as documented in the 1.1 handbook. A few additions round the syntax out for real project work.

### What Counts as False, a Reminder

As in 1.1: `false`, `null` and `0` are false, and everything else is true, an empty string and an empty list included. `if mouse.left then` and `if gamepad.A then` read the way they look, because microStudio's input says 1 and 0. The checker adds one thing: an `if` on a value that can only ever be a string or a list, which is always true, is a warning.

### Modulo, a Reminder

```tiberscript
if score % 100 == 0 then
  sfx("milestone")
end
```

`%` has been in 1.1 since its last update, and is repeated here because 1.2 leans on it. It returns the remainder of a division. Useful for anything that repeats on a cycle: every 10th enemy, every 4th frame, wrapping a counter around so it never grows without bound. It wraps the way a game wants: `-1 % 4` is `3`, not `-1`, so an index that steps below zero lands at the end of the list instead of off it. `%=` works like `+=`.

### String Escapes, a Reminder

```tiberscript
line = "She said \"go\" and left.\nThen it was quiet."
```

`\"` for a quote inside a string, `\n` for a line break, `\\` for a literal backslash, and `'single quotes'` for a string with double quotes in it. All of these work in 1.1 already. 1.2 adds one escape, `\$`, for the interpolation below.

### String Interpolation

```tiberscript
screen.text("Score: $(score)   Best: $(best)", 0, 90, 12, "white")
msg = "$(player.name) has $(player.hp) of $(player.maxHp) hp"
```

`$(anything)` inside a string is replaced by the value of what is between the parentheses, when the string is used. It is the same `$` that `string.format` already uses, so there is one mark for "a value goes here" in the whole language. Write `\$` for a literal dollar sign.

### A data Block in a local

```tiberscript
makeSpark(px, py) = function
  local spark = data
    x = px
    y = py
    life = 30
  end
  return spark
end

init() = function
  first = makeSpark(10, 20)
  print(first.life)
end
```

A `data` block can be a `local` inside a function too, written over several lines the way it is at the top of a file, the same as `name = data` and `this.name = data`. The one line form, `local spark = data(x = px, y = py)`, works as well.

### Inline Functions

Functions have always been values: you pass them to `Tiber.addCommand()`, `array.filter()`, and `.connect()`. Until now every one of them needed its own name and its own definition somewhere else in the file, even a two line callback used exactly once. 1.2 lets you write one where you use it:

```tiberscript
alive = array.filter(enemies, function(e)
  return e.hp > 0
end)

player.died.connect(function()
  transit(states.gameOver)
end)
```

An inline function is written exactly like a named one, minus the name and the `=`: the word `function`, its parameters in parentheses, its body, `end`. It can read the variables around it at the moment it runs. Named functions are still the better choice for anything used twice, and nothing in the standard library needs inline ones: every helper takes a named function just as happily.

### Loop Counters Belong to Their Loop

```tiberscript
for i = 1 to 10 do
  spawnCoin(i)
end
```

In 1.1 the `i` above was an ordinary global, which meant a function called from inside the loop that also used `i` for its own loop would quietly change it. That is a real bug, and it hides well. In 1.2 a `for` loop's counter, and a `for x in list` loop's `x`, exist only inside that loop. A global of the same name, or a `local` of the same name declared before the loop, is left exactly as it was when the loop ends. This is the one change in 1.2 that can affect a 1.1 script, and only one that reads a loop's counter after the loop has finished. If you need the last value, copy it into your own variable inside the loop. Code that must keep the old rule can keep it: inside `tiberscript 1.1 ... end` a counter is an ordinary name again (see TiberScript 1.0 and 1.1 Inside 1.2, at the end of this section).

### Time in Seconds

```tiberscript
sequence do
  wait(seconds(1.5))
end
every(seconds(2)) do
  spawnSlime()
end
schedule(seconds(3)) = function
  respawnPlayer()
end
```

Everything that counts frames also accepts `seconds(n)`, which is `n` times the game's tick rate. A game written in seconds keeps its timing if the tick rate ever changes.

### Values a Parameter Has of Its Own

```tiberscript
spawn(x, y, speed = 1, hp = x * 10) = function
  return [x, y, speed, hp]
end

init() = function
  a = spawn(2, 3)
  b = spawn(2, 3, 5)
  print(a)
  print(b)
end
```

A parameter can say what it is when a call leaves it out: `speed = 1`. The value is worked out each time it is needed, and it can use the parameters before it, so `hp = x * 10` follows whatever `x` was given. `spawn(2, 3)` is `[2, 3, 1, 20]` and `spawn(2, 3, 5)` is `[2, 3, 5, 20]`. A value that is given, `null` included, is used as it is. Methods (`init()` too) and inline functions take values of their own the same way.

### Values Given by Name

```tiberscript
hit(target, amount = 1, times = 1, element = "none") = function
  return target + " takes " + amount * times + " " + element
end

init() = function
  a = hit("slime", times = 3)
  b = hit("bat", element = "fire", amount = 2)
  print(a)
  print(b)
end
```

A value can be given by its parameter's name, after the ones given in their places, in any order: `hit("bat", element = "fire", amount = 2)` is `"bat takes 2 fire"`, and the parameters it does not name keep their own values. It reads like a sentence, and a call with four numbers in a row no longer needs the reader to count them. Functions, methods (`hero.hit(times = 3)`), `new` (`new hero(hp = 40)`) and inline functions all take values by name.

A name that is not one of the function's parameters is given the way 1.1 gave it: together with any others like it, as one value with a field for each, in the first place left. Built ins that take named values, like `physics.add(crate, "box", 16, 16, bounce = 0.5)`, read them exactly as before. Giving the same value twice, in its place and by name, stops with `'amount' was given twice: once in its place, and once by name`.

### elseif

```tiberscript
grade(score) = function
  if score >= 90 then
    return "A"
  elseif score >= 80 then // elseif or elsetry: either one is fine
    return "B"
  elsetry score >= 70 then
    return "C"
  else
    return "D"
  end
end
```

`elseif` is another way to write `elsetry`. Either one is fine, the way `archetype` and `class` are: a game can use the one it likes, or both. `else if`, as two words, is neither, and the Forge says so with its line.

### match

```tiberscript
size(v) = function
  match v
    case 1, 2 then return "small"
    case 3 to 9 then return "middle"
    case "boss"
      local s = "huge"
      return s + "!"
    else
      return "other"
  end
end

init() = function
  print(size(2))
  print(size(7))
  print(size("boss"))
end
```

`match` picks one case for a value. A case can list several values, `3 to 9` takes every number from 3 to 9 with both ends included, a case on one line says `then`, a case with more to do puts it on the lines below, and `else` takes everything no case did. The cases are tried from the top, the first that fits runs, and the rest are skipped: nothing falls through into the case below. A value is compared the way `==` compares, so a case can be a number, a text, `true`, `false`, `null` or an enum's state (`case states.run, states.jump then`).

`match` reads a long `if ... elsetry ... elsetry` (or `elseif`) as the list of choices it is. `match` is still a name a game may use for its own values: it starts a statement only when a value follows it on the same line, and a `match` without `case` lines says how one reads.

### const

```tiberscript
const MAX_LIVES = 3
const GRAVITY = 0.35

init() = function
  lives = MAX_LIVES
  fall = GRAVITY * 2
end
```

`const` gives a name a value for good, at the top of a file. It reads like any other name. Giving it another value anywhere stops the game with the line where its value was given: `'MAX_LIVES' is a constant: it keeps the value it was given at line 1`, and giving a constant twice in one file says so as the file loads. A constant is the place for the numbers a game is built around. A value you want to change while the game runs is a tunable instead (`export`, Section 4).

### A Value Chosen by a Condition

```tiberscript
init() = function
  hp = 0
  status = "alive" if hp > 0 else "down"
  medal = "gold" if score > 900 else "silver" if score > 500 else "bronze"
end
```

`value if condition else other` is one value: the first when the condition is true, the other when it is not, and only the one chosen is worked out. A chain reads from the left, the way the second line above does. The `else` is always there; without it, the line says what it needs. It works in shaders too, and in `fast` code, where both values are numbers.

### otherwise

```tiberscript
init() = function
  saved = null
  name = saved otherwise "Player"
  times = [12.5, 9.8]
  third = times[2] otherwise 0
end
```

`a otherwise b` is `a`, unless `a` is `null`, and then it is `b`, which is worked out only then. Only `null` counts: `0`, `false` and `""` stay as they are, which is what a score, a setting or a name that may really be zero or empty needs. It is the short way to give a value its default.

### in and not in

```tiberscript
init() = function
  inventory = ["key", "map"]
  if "key" in inventory then
    print("the door opens")
  end
  if "sword" not in inventory then
    print("unarmed")
  end
  hasX = "x" in data(x = 1, y = 2)
  says = "ell" in "hello"
end
```

`in` asks whether a list holds a value, whether a text holds a smaller text, or whether a `data` value has a field of that name. `not in` asks the opposite. It is `.contains()` written the way the sentence is said.

### Two Names in a Loop

```tiberscript
init() = function
  stats = data(hp = 100, mp = 40, xp = 0)
  for name, value in stats do
    print(name + " is " + value)
  end
  for i, item in ["sword", "shield", "potion"] where i > 0 do
    print(i + ": " + item)
  end
end
```

Over a `data` value, two names give each field's name and its value, in the order the fields were made. Over a list, they give each place, counted from 0, and the item in it. `where` works with two names the way it does with one, and both names belong to the loop, the way a single one does.

### A Game's Own Function Comes First

1.2 gains built in functions as it grows: `approach()`, `remap()`, `wrap()`, `ease()` and the rest of Section 2 came after the first games were written in it. A game that already had a function of the same name keeps it. When a game defines its own `ease()`, every call in that game is the game's own, and the built in waits unused; the same goes inside `each fast`. Nothing a game wrote changes meaning because the language grew around it. These are the built ins a game's own function comes before: `approach`, `remap`, `wrap`, `ease`, `chance`, `weightedChoice`, `cooldown`, `cooldownLeft`, `type`, `inspect`, `tr`, `grid`, `actionAxis`, `actionVector`, `springTo`, `watch` and `repeats`.

Every other built in answers to its own name first: `clamp()`, `lerp()`, `floor()`, `sfx()`, `shake()`, `flash()`, `print()` and the rest. A game's own function with one of those names is never called, so give it a name of its own. When the game starts in the editor, the checks of Section 12 name that function and its line.

Inside a module (Section 3) a library's own functions come first for every name: a library that has its own `clamp()` or its own `tween()` calls its own from its own code, and the game that imports it keeps the built in.

### TiberScript 1.0 and 1.1 Inside 1.2

```tiberscript
tiberscript 1.1
  countUp() = function
    for i = 1 to 3 do
    end
    return i
  end
end

init() = function
  last = countUp()
  which = Tiber.version
end
```

`tiberscript 1.1` opens a block and `end` closes it, and everything between the two is read by the rules of TiberScript 1.1. `last` is `3`: in 1.1 a loop's counter is an ordinary name that keeps its last value, and the function was written counting on it. Outside the block the game is 1.2, and `which` is `"1.2"`. `tiberscript 1.0 ... end` does the same with the rules of 1.0.

Every 1.1 program already runs on 1.2, so the rules that differ are few, and they are exactly the places where 1.2 could surprise code written before it:

- **A loop's counter is a name like any other.** It is the function's `local` of that name if there is one, a global otherwise, and after the loop it holds the last value it had.
- **What came later gives way.** A built in function that the old version did not have (`seconds()`, `vector()`, `approach()` and the rest) is used only when the game has nothing of its own by that name, even a function it makes while it runs.
- **A name the engine took later is the game's own.** 1.2 has `time`, `camera`, `save`, `effects` and more. A 1.1 game that counted its own `time = time + 1`, or kept its own `camera`, keeps doing exactly that inside the block. In a 1.0 block the same goes for what 1.1 added, `mouse` and `shake()` among them. It also means the engine's own `time` and `camera` are not reached from inside an old block: code that wants them goes outside it, or in a `tiberscript 1.2 ... end` block of its own.

`Tiber.version` says which rules the code it is read in follows: `"1.1"` inside the block above, `"1.2"` outside it. A function, an archetype or a method written inside a block keeps the block's rules whoever calls it, from anywhere. Blocks sit inside blocks: `tiberscript 1.2 ... end` inside old code is 1.2 again, and a `javascript` block (19.19) works in every one of them.

A whole file is marked with one line:

```tiberscript
Tiber.version = "1.1"

steps = 0
walk() = function
  for k = 1 to 4 do
    steps += k
  end
  return k
end
```

`Tiber.version = "1.1"` on a line of its own, not indented, reads everything below it in that file by the 1.1 rules: `walk()` gives `4`. It is the way to bring an old game's file into a new project without touching a line of it; the other files stay 1.2. A 1.0 game written as nothing but `during` blocks works the same way under `Tiber.version = "1.0"` or inside `tiberscript 1.0 ... end` at the top of a file: its `during` blocks run every frame, as they did.

Inside a function, `Tiber.version = "1.1"` changes nothing about how code is read; it only changes what `Tiber.version` says from then on. A version that is none of the three stops the file as it loads, with `TiberScript has the versions 1.0, 1.1 and 1.2`. And `tiberscript` is still a name a game may use for a value of its own: it opens a block only with a version after it.

A project that is itself set to TiberScript 1.1 in its settings is a different thing: it runs on the 1.1 engine, kept frozen. The blocks are for 1.0 and 1.1 code that lives inside a 1.2 project.

---

## 2. The Standard Library

1.1 gives you `.length`, `.charAt()`, `.indexOf()`, `.startsWith()`, `.endsWith()`, `.contains()`, `.toUpper()`, `.toLower()`, `.trim()`, `.substring()`, `.replace()` and `.split()` on strings, and `.push()`, `.length`, `.removeAt()`, `.insert()`, `.pop()`, `.contains()`, `.indexOf()`, `.join()`, `.sort()` and `.reverse()` on lists. 1.2 adds the helpers that need more than one piece of information, a function or a second list, and real `vector`, `math` and `table` namespaces, so common logic does not have to be hand written from scratch in every project.

Functions are already values in TiberScript. You have been passing them to `Tiber.addCommand()` since 1.1. That means the array helpers below take a plain function reference, named or inline.

### string

```tiberscript
name = "  Hero  "
trimmed = name.trim()
parts = "sword,shield,potion".split(",")
joined = string.join(parts, " | ")
msg = string.format("Score: $0 / $1", score, maxScore)
loud = "hello".toUpper()
quiet = "HELLO".toLower()
padded = "7".padLeft(3, "0")
```

`trimmed` is `"Hero"`. `parts` is `["sword", "shield", "potion"]`. `joined` is `"sword | shield | potion"`, the same as `parts.join(" | ")` from 1.1. `string.format` fills in `$0`, `$1`, and so on with the arguments that follow it, in order, left to right. `padded` is `"007"`, which is the kind of thing a score display or a timer needs constantly and had no clean way to do in 1.1 without manual string building.

Methods that act on one specific string, like `.trim()` and `.split()`, stay dot methods on that string, exactly like `.length` already worked in 1.1. `string.format()` is namespaced instead, because it takes more than one value as input and does not belong to any single one of them. `string.join(list, sep)` is the same, and `list.join(sep)` stays as the short way to say it.

### array

```tiberscript
isAlive(e) = function
  return e.hp > 0
end

byDistance(a, b) = function
  return distance(player.x, player.y, a.x, a.y) < distance(player.x, player.y, b.x, b.y)
end

survivors = array.filter(enemies, isAlive)
closest = array.sort(enemies, byDistance)
found = array.find(inventory, isKey)
hasKey = array.contains(inventory, "key")
```

```tiberscript
init() = function
  seedRandom(7)
  deck = array.shuffle([1, 2, 3, 4, 5, 6])
  total = array.sum([3, 4, 5])
  mean = array.average([2, 4, 9])
  kinds = array.unique(["slime", "bat", "slime"])
  top = array.first(deck)
  bottom = array.last(deck)
end
```

`array.shuffle(list)` gives a shuffled copy, from the game's own random numbers, so a seeded game shuffles the same way every time and a rewind shuffles again the same. `array.sum()` and `array.average()` add up numbers, `array.unique()` keeps each item once, in the order it first came, and `array.first()` and `array.last()` are an end of a list, or `null` for an empty one.

`array.filter` keeps only the items a function returns true for. `array.sort` takes a comparison function, the same shape you would write for any two items in the list, and returns a new sorted list without changing the old one; 1.1's `list.sort()` still sorts a list of numbers or strings in place, with no function. `array.find` returns the first match, or `null` if nothing matched, the same way `collide()` already does. `array.contains(list, x)` is the same as 1.1's `list.contains(x)`, kept so a function that is handed a list can be written either way. The namespaced helpers exist because each one needs a function, and a function reads best as the second argument, not tucked inside a dot. For "some of the items" there is also `where`, in Section 19, which picks items out inside the loop itself.

### vector

```tiberscript
v = vector(3, 4)
len = v.length()
dir = v.normalize()
sum = vector(1, 2).add(vector(3, 4))
angle = math.atan2(dy, dx)
lerped = math.lerp(0, 100, 0.5)
```

`v.length()` is `5`. `vector` gives you a real point or direction with its own methods, instead of tracking an x and a y as two separate numbers everywhere and writing the same distance and angle math by hand in every project. It is lowercase for the same reason `data` and every archetype name already are: it makes a value, the same way `data` does, and nothing in TiberScript that makes a value rather than defining a piece of the engine itself is capitalized.

A vector never changes: `.add()`, `.scale()`, and `.normalize()` return a new one. That is what makes it safe to pass around, and it is what lets the Forge (Section 19) turn vector math into plain arithmetic with no objects made at all.

### math

`math.atan2()` and `math.lerp()` above are new namespaced helpers. `random()`, `clamp()`, `wave()`, `distance()`, `min()`, `max()`, `abs()`, `sqrt()`, `floor()`, and `ceil()` all stay exactly as they were in 1.1: plain functions, not part of the `math` namespace, so no 1.1 script needs to change. `math.lerp(from, to, amount)` blends smoothly between two numbers. It is what `camera.follow()` and tween easing further down both use internally, and it is genuinely useful on its own for anything that should ease toward a value rather than jump to it.

### table

`data` blocks are fixed shape. You always know the field names ahead of time. `table` is for when you do not, which comes up constantly once a game has an inventory, a save file, or anything else shaped by what the player has actually done.

```tiberscript
for key in table.keys(config)
  print(key + " = " + config[key])
end

value = table.get(config, "difficulty", "normal")
table.set(config, "difficulty", "hard")
hasIt = table.has(config, "lootTable")
table.remove(config, "lootTable")
copyOf = table.copy(config)
```

`table.get` takes a default, the same as `save.get` further down, so a missing key never crashes your game. `table.remove` deletes a key entirely, which a plain `data` block has no way to do at all since its shape is meant to stay fixed. `table.copy` makes an independent copy, so changing the copy never changes the original.

### Game Feel: approach, remap, wrap, round and ease

```tiberscript
init() = function
  speed = 0
  speed = approach(speed, 6, 0.5)
  percent = remap(45, 0, 90, 0, 100)
  angle = wrap(370, 0, 360)
  shown = round(2.567, 2)
  y = ease("bounce", 0.5) * 80
end
```

These are the small pieces of math every game writes again and again, here once:

- `approach(value, target, step)` moves a value toward a target by at most `step`, and never past it: a speed that eases up to its top, a bar that drains, a camera that settles. `approach(0, 6, 0.5)` is `0.5`, and from `5.8` it is `6`, never `6.3`.
- `remap(value, from1, to1, from2, to2)` moves a value from one range to another: a health of 45 out of 90 is 50 percent, a stick's tilt is a speed, a distance is a volume.
- `wrap(value, low, high)` keeps a value between low and high by going round: 370 degrees is 10, a ship that leaves the right edge comes back on the left, and an index below 0 counts from the end.
- `round(n, digits)` rounds to that many digits after the point, for a number shown on screen; `round(n)` is still a whole number.
- `ease(name, t)` gives `t`, from 0 to 1, along a curve: `linear`, `easeIn`, `easeOut`, `easeInOut`, `bounce`, `elastic`, `back` (a little past, then back), `anticipate` (a little back first), `smooth`, `sine` and `expo`. They are the curves `tween()` uses (Section 8), as plain numbers, for anything `tween()` does not move.

`approach()`, `remap()`, `wrap()` and `round(n)` work in `fast` code too, and give exactly what they give outside it.

### Springs: springTo()

`approach()` reaches its target and stops. A spring reaches it, goes a little past, comes back, and settles: the movement of anything that has weight. `springTo(object, "field", target)` moves one field of an object one step toward a target that way, each time it is called, and gives back the new value. Call it once a tick.

```tiberscript
init() = function
  button = data
    size = 1
  end
  panel = data
    x = -150
  end
end

update() = function
  if key("space").hit then
    button.size = 1.5
  end
  springTo(button, "size", 1)
  springTo(panel, "x", 0, stiffness = 0.08, damping = 0.85)
end

draw() = function
  screen.clear("#101020")
  screen.rect(panel.x, 40, 160, 30, "#262E4E")
  screen.rect(0, -20, 60 * button.size, 24 * button.size, "#3C4A78")
  screen.text("PLAY", 0, -20, 10 * button.size, "#FFFFFF")
end
```

The button is knocked to one and a half times its size by a key and springs back to 1, a little under it and back, in a third of a second. The panel slides in from the left, softly, and settles.

| Option | What it sets | When left out |
| --- | --- | --- |
| `stiffness` | how hard the spring pulls toward the target, from 0 to 1: higher is quicker | 0.2 |
| `damping` | how much of its speed it keeps each tick, from 0 to 1: higher swings longer, lower settles sooner | 0.75 |

The spring's speed is kept on the object itself, in a field named after the first with `Speed` on the end (`button.sizeSpeed`, `panel.xSpeed`). That is all there is to a spring: two numbers on an object. So a spring is saved with `save.game()`, rewound, forecast and replayed like any other value, with nothing extra to do, and setting the speed field by hand gives it a kick. It moves at the same pace at 60 and at 120 updates a second.

Springs are for what the eye follows, not for what the rules depend on: a size, a tilt, a camera's zoom, a number counting up on a HUD. For a hero's own position, physics (Section 11, Section 24) is the tool.

### Cooldowns

```tiberscript
fixedUpdate() = function
  if action("fire").held and cooldown("fire", 12) then
    shoot()
  end
end

shoot() = function
  print("bang, ready again in " + cooldownLeft("fire") + " ticks")
end
```

`cooldown(name, ticks)` is `true` when that cooldown is ready, and then starts it again: here a shot every 12 ticks at most, however fast the button is pressed. `cooldownLeft(name)` is how many ticks are left, for an icon that fills up. A cooldown counts ticks of the world, so it rewinds, replays and rolls back with everything else: a rewound shot is ready again exactly when it was.

### Odds: chance and weightedChoice

```tiberscript
init() = function
  seedRandom(3)
  if chance(0.25) then
    print("a critical hit")
  end
  loot = weightedChoice(["coin", "gem", "crown"], [80, 18, 2])
end
```

`chance(p)` is `true` with a chance of `p`, from 0 to 1. `weightedChoice(items, weights)` picks one item, each as likely as its weight: a coin 80 times in 100, a crown twice. Both use the game's own random numbers, so `seedRandom()` makes them the same every time, and the Timeline replays them exactly.

### A Grid

```tiberscript
init() = function
  board = grid(8, 6, 0)
  board.set(3, 2, 1)
  here = board.get(3, 2)
  off = board.get(20, 20)
  around = board.neighbors(3, 2, true)
  filled = board.count(1)
end
```

`grid(width, height, fill)` is a grid of values, every place starting as `fill`: a board, a level's tiles, a field of life. `.get(x, y)` is the value at a place, `null` off the grid. `.set(x, y, value)` changes one, `.inside(x, y)` asks whether a place is on the grid, `.fill(value)` sets every place, `.count(value)` counts the places that hold it, and `.neighbors(x, y)` is the places next to one, as vectors, four of them, or eight with `.neighbors(x, y, true)`, leaving out any off the edge. A grid is a TiberScript value like any other, so the Timeline keeps it and `save.game()` saves it.

### type() and inspect()

```tiberscript
hero2 = archetype
  init() = function
    this.hp = 3
  end
end

init() = function
  kinds = [type(1), type("x"), type([1]), type(null), type(new hero2())]
  print(inspect(data(name = "Ada", pos = vector(3, 4))))
end
```

`type(value)` names what a value is: `"number"`, `"text"`, `"list"`, `"bool"`, `"null"`, `"vector"`, `"function"`, `"buffer"` (19.23), `"object"` for a `data` value, or an archetype's own name for an instance of it. `inspect(value)` writes a whole value out as text, the way you would write it, fields and lists and all, for `print()` and the console: `data(name = "Ada", pos = vector(3, 4))`.

### Translations: tr() and Tiber.language

```tiberscript
translations = data
  en = data(hello = "Hello, \$0!", play = "Play")
  fr = data(hello = "Bonjour, \$0 !", play = "Jouer")
end

init() = function
  Tiber.language = "fr"
  greeting = tr("hello", "Ada")
  button = tr("play")
end
```

A game in more than one language keeps its texts in a `data` value named `translations`, one field per language, each a `data` of texts by key. `tr(key, ...)` gives the text for that key in the player's language, with `$0`, `$1` and on filled in with the values after the key (written `\$0` inside the text, so it is not read as interpolation); a text the player's language does not have comes from English, and a key nobody has is shown as itself, so a missing text is easy to spot and never an error. `Tiber.language` is the player's language, two letters: it starts as the browser's own, and the game can change it, from a settings menu for example.

### Tiber.translate: Every Text in Another Language

```tiberscript
translations = data
  fr = data(play = "Jouer")
end

init() = function
  translations.fr["SCORE #1"] = "POINTS #1"
  translations.fr["PRESS START"] = "APPUIE SUR START"
  Tiber.translate = "fr"
  score = 120
end

draw() = function
  screen.clear()
  screen.text("SCORE $(score)", 0, 20, 12, "#FFD750")
  screen.text("PRESS START", 0, 0, 10, "#FFFFFF")
end
```

`tr()` is for texts a game names by key. `Tiber.translate` is for all the rest, with no change to the code that draws them: every text the game draws with `screen.text()` or `screen.textOutline()` is drawn in that language. This game shows `POINTS 120` and `APPUIE SUR START`.

- **`Tiber.translate = "fr"`** and **`Tiber.translate("fr")`** do the same. A language is two letters (`"ja"`, `"es"`, `"de"`) or its name in English (`"japanese"`, `"spanish"`). `"auto"` is the player's own language (`Tiber.language`), and `"off"` puts every text back as written. `Tiber.translate` printed or put in a text says the language it is set to, or `off`.
- **A text keeps its place, its color, its font and its size.** A translation longer than the text it replaces is drawn smaller, just enough to fit in the same width (never under a little less than half the size), so a menu, a score and a dialogue box keep their layout.
- **Numbers stay numbers.** `"SCORE 120"` is looked up as `"SCORE #1"` (`#1` stands for a text's first number, `#2` for its second, and on), and the 120 is put back in, so a score that counts up is one text to translate, not a new one every frame.
- **`Tiber.translated(text)`** gives one text as the game would show it now, and `Tiber.translateFrom` is the language the game's texts are written in (`"en"` unless it says otherwise).

The words come from four places, in this order. The game's own `translations`, a text found by what it says (`translations.fr["PRESS START"]`). Then the project's file `assets/tiber_translations.json`: every translation the game showed while it ran in the editor is kept there, so an exported game has them in every browser, with no network. Then what this browser translated before. And last, where a browser has a translator of its own, that translator, asked once a text in the background; until it answers, the text shows as written. Where none of the four has a text, it is drawn the way the game wrote it, and the console says why once.


---

### colors

A color in TiberScript is a text: `"#FF8800"`, `"#F80"`, `"rgb(255, 136, 0)"`, `"rgba(255, 136, 0, 0.5)"`, or a plain name such as `"white"`. `colors` makes one color from another, so a game can keep a few colors and work the rest out.

```tiberscript
init() = function
  base = "#3A7BD5"
  hp = 30
end

draw() = function
  screen.clear(colors.darken(base, 0.7))
  screen.rect(0, 60, 120, 24, base)
  screen.rect(0, 30, 120, 24, colors.lighten(base, 0.3))
  screen.rect(0, 0, 120, 24, colors.fade(base, 0.4))
  local bar = colors.mix("#FF5050", "#78DC8C", hp / 100)
  screen.rect(0, -40, 100, 10, bar)
  local label = "#FFFFFF"
  if colors.brightness(bar) > 0.6 then
    label = "#000000"
  end
  screen.text("hp " + hp, 0, -40, 7, label)
end
```

One blue gives the background (much darker), a lighter band, and a band that lets the background through. The bar is somewhere between red and green, by how much health is left, and its label is white or black, whichever reads on it.

| Function | What it gives |
| --- | --- |
| `colors.mix(a, b, amount)` | the color between two: 0 is `a`, 1 is `b`, and half way when no amount is given |
| `colors.lighten(color, amount)` | the color brought toward white, by 0.2 when no amount is given |
| `colors.darken(color, amount)` | the color brought toward black, by 0.2 when no amount is given |
| `colors.fade(color, alpha)` | the color made see through: 0 is not there at all, 1 is solid |
| `colors.rgb(red, green, blue, alpha)` | a color from three numbers of 0 to 255, and how solid it is from 0 to 1 when a fourth is given |
| `colors.hsl(hue, strength, lightness, alpha)` | a color from a hue in degrees (0 red, 120 green, 240 blue), how strong it is from 0 to 1, and how light from 0 to 1 |
| `colors.parts(color)` | the color as a list of four numbers: red, green, blue (0 to 255) and how solid (0 to 1) |
| `colors.brightness(color)` | how bright the color looks to the eye, from 0 (black) to 1 (white) |
| `colors.valid(text)` | `true` when the text is a color |

What comes back is a text like any other color: `"#90A0B0"` for a solid one, and `"rgba(255,136,0,0.4)"` for one that is see through. It goes anywhere a color goes: `screen.rect()`, `alter color=`, a light's color, a particle's.

`colors.hsl()` is the one for a color that moves: turn the hue with the frame count and a pickup runs through the rainbow, and keep the hue and change the lightness and it pulses.

```tiberscript
init() = function
  turn = 0
end

update() = function
  turn = (turn + 2) % 360
end

draw() = function
  screen.clear("#101020")
  for i = 0 to 11 do
    screen.circle(-110 + i * 20, 0, 8, colors.hsl(turn + i * 30, 0.8, 0.6))
  end
end
```

A text that is not a color is an error that says what a color looks like, so a typing mistake in a color is found on the line it is on.

## 3. Modules and The Import System

This is the biggest addition to how a project is organized in 1.2. `Tiber.addScript()` from 1.1 still works exactly as before. It is not going anywhere. It was always a workaround though, built to solve the same problem a real module system solves properly: one script cannot hold an entire full length game.

```tiberscript
// enemies.tbs
export damageable = trait
  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then this.die() end
  end
end

export slime = archetype uses damageable
  init(x, y) = function
    this.x = x
    this.y = y
    this.hp = 30
    this.mark("enemy")
  end
  die() = function
    this.vanish()
  end
end

export goblin = archetype extends slime
  init(x, y) = function
    super.init(x, y)
    this.hp = 40
  end
end
```

```tiberscript
// main.tbs
import "enemies"
import "ui/healthbar"

init() = function
  boss = new enemies.goblin(0, 0)
end
```

`export` in front of any top level definition makes it visible to files that `import` this one. `import "filename"` gives you that whole file as a namespace matching the filename: `enemies.slime`, `enemies.goblin`, `enemies.damageable`. Anything not marked `export` stays private to its own file, which means two files can each have their own local helper function with the same name and never collide, something that was genuinely impossible in a single file 1.1 project of any real size.

Imports are resolved before the game starts, not while it runs. That is why an `import` always sits at the top of a file and never inside an `if`: the engine reads every file's imports first, loads each file once, in the right order, whichever file the editor happens to hand over first, and tells you by name if two files import each other in a circle. A missing file is an error the moment you press Run, not three levels into the game.

**A value a module exports is shared, not copied.**

```tiberscript
// rules.tbs
export gravity = 9.8
export const LIMIT = 3

export heavier() = function
  gravity += 1
  return gravity
end
```

```tiberscript
// main.tbs
import "rules"

init() = function
  rules.gravity = 20
  now = rules.heavier()
  most = rules.LIMIT
end
```

`now` is `21`. The module and the file that imports it see one value: `rules.gravity = 20` changes it for the module's own code too, and what `heavier()` then does to it is what `rules.gravity` reads. That is what lets a library keep its settings as plain exported values (`lib.gravity`, `lib.debug`) that a game simply sets. An exported `const` can be read from anywhere and never given another value: `rules.LIMIT = 4` stops with `'LIMIT' is a constant of rules`.

**A module's own functions come first, inside it.** A library is free to name a function the way it reads best, `clamp()` or `tween()`, even where the language has a built in of that name. The module's own code calls the module's own, the game calls it as `kit.clamp()`, and everywhere else the built in is untouched.

**A module's `init()`, `update()` and `draw()` are its own.** A library with a start and a frame of its own exports them like anything else, and the game calls them when it wants them: `kit.init()`, `kit.update()`, `kit.draw()`. They never take the place of the game's own `init()`, `update()` and `draw()`, in whatever order the files are read.

**`export javascript ... end`** exports what a block of JavaScript declares, its functions, classes and values, the same way (19.19).

### Tiber Libraries: Yours, in Every Project

A module belongs to its project. A Tiber library is a module of yours that every one of your TiberScript projects can use, kept on your own computer.

```tiberscript
// tools.tbs, in any one of your projects
Tiber.create("tools")

export double(n) = function
  return n * 2
end

export TOP = 9
```

```tiberscript
// main.tbs, in any other of your projects
init() = function
  if Tiber.tools != null then
    best = Tiber.tools.double(Tiber.tools.TOP)
  end
end
```

- **`Tiber.create("tools")`**, or `Tiber.create = "tools"`, in a file of a project: when that game runs in the editor, the file is kept as the library `tools`, under your name, on the computer microStudio runs on. Run it again after a change and the library is kept again, with the change. The name is letters, digits and `_`, starting with a letter, and a name the engine already has (`Tiber.version`, `Tiber.translate`) is refused, with a message that says so.
- **`Tiber.tools`** in any of your TiberScript projects is what the library exports, read the first time it is used: its functions, its values, its archetypes. What it does not export stays inside it, as in any module.
- **`Tiber.libraries`** is the list of them: each one's `name`, what it `exports`, and the `project` it was kept from.
- **In the editor**, the library panel has a part called Your Tiber Libraries: each library with what it exports. A click copies `Tiber.tools.` to paste into your code, and Forget removes the library.
- **An exported game** carries every library its code names, so it runs anywhere, with no library kept anywhere else.

A library is read only for its owner and that owner's projects. Nothing about it leaves the computer.

Sub scripts and modules solve different problems. Sub scripts are for splitting up one big `during` block into named, callable pieces without leaving that block. Modules are for genuinely separate, reusable files that make sense as their own thing, like an enemies file, a UI file, or a save system file. Use whichever one actually fits what you are doing. Neither one is the deprecated option.

---

## 4. Object Oriented Programming (Archetypes)

`archetype`, `class`, `init`, `extends`, `super`, `new`, and `mark()` inheritance all work exactly as in 1.1.

### Traits

`extends` is single inheritance. One parent, one line of ancestry. Real games constantly need to share behavior across unrelated branches of that tree. A player and a destructible crate are nothing alike, but both might need to take damage. That is what a trait is for.

```tiberscript
damageable = trait
  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then this.die() end
  end
end

flammable = trait
  ignite() = function
    this.burning = true
  end
end

crate = archetype uses damageable and flammable
  init() = function
    this.hp = 20
  end
  die() = function
    this.vanish()
  end
end
```

Chain as many traits as the archetype actually needs with `and`:

```tiberscript
hero = archetype uses damageable and flammable and swimmable and climbable and freezable
  init() = function
    this.hp = 100
  end
  die() = function
    transit(states.gameOver)
  end
end
```

An archetype can `extends` one parent and `uses` any number of traits at the same time. Traits cannot be created with `new` on their own. They only exist to be mixed into an archetype. If an archetype defines a method a trait also defines, the archetype's own definition always wins, the same way a child overriding a parent already works with `extends`. If two traits on the same archetype define the same method and the archetype does not settle it by defining its own, that is an error that names both traits and the method, because silently picking one of them is exactly the kind of bug traits exist to prevent.

### Tunable Variables

Every number in a game gets tuned by hand dozens of times before it feels right: how fast the player moves, how much damage an attack does, how long an enemy takes to respawn. Doing that by hunting through code every time is slow. `export` marks a value as something meant to be adjusted from outside the script entirely.

```tiberscript
enemy = archetype
  export moveSpeed = 2
  export attackDamage = 10

  init() = function
    this.hp = 50
  end
end
```

An exported value still works exactly like a normal field inside the script. The difference is that it also shows up in a dedicated panel in the editor, so tuning enemy speed or damage is a slider, not a search through every file that mentions it. Changes made in the panel apply while the game runs, and a button writes them back into the code when you like them.

The panel is the sliders button on the run window's bar, there whenever a TiberScript 1.2 game is running. Each exported number has a slider and a box to type in; a change goes straight into the running game, into the archetype's default and every instance already alive. The arrow beside a value writes it into the code, on the line that exports it, and **write all to code** does every value you changed. From code, `Tiber.tunables()` lists them (name, value, file, line) and `Tiber.setTunable(name, value)` changes one.

This is the one place in this whole handbook where the word `export` means something different from Section 3. There, `export` marks a definition as visible to other files. Here, it marks a single value as editable from outside code entirely. Both uses share the word because both are about making something reachable from outside the script that defines it, just at different scopes, and the rule for telling them apart is simple: at the top of a file, `export` shares a definition with other files; inside an archetype or a `data` block, it shares a value with the editor.

---

## 5. The Game Flow (State Management)

`transit()` and `during` are unchanged. Three additions make bigger state machines safer and easier to sequence.

### Enums

State names have always been plain strings. A typo in `transit("mnue")` fails silently at runtime with no warning at all, and the screen just goes blank with no clue why. `enum` gives you named, checkable constants instead.

```tiberscript
states = enum
  menu
  game
  paused
  gameOver
end

init() = function
  transit(states.menu)
end

update() = function
  during states.menu do
    if key("space").hit then transit(states.game) end
  end
end
```

`states.mnue` is now a real error the moment it is written. It is not a game that silently goes blank three states later with no explanation.

### Entering and Leaving

A `during` block runs every frame the state is active. Some things should happen exactly once: start the music when a level begins, save the high score when the game ends. In 1.1 that meant a flag variable and an `if` inside the `during` block, and forgetting the flag meant saving sixty times a second.

```tiberscript
entering states.gameOver do
  silence("music")
  if score > best then
    best = score
    save.set("highScore", best)
    save.write()
  end
end

leaving states.game do
  audio.setVolume("sfx", 1)
end
```

`entering` runs once, on the frame `transit()` switches into that state, before its first `during`. `leaving` runs once, on the frame a `transit()` switches away from it. Both can appear anywhere a `during` can.

### Sequences

Cutscenes, intros, and timed attack patterns used to mean building a manual counter and a pile of `if timer == 60 then ... end` checks scattered across a `during` block. `sequence` lets you write it top to bottom, in order, like a script, which is exactly what it actually is.

```tiberscript
entering states.bossIntro do
  sequence do
    caption = "A wild boss appears!"
    wait(60)
    shake(6, 20)
    sfx("roar")
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

`wait(N)` pauses just that sequence for N frames. The rest of your game, including `update()` itself, keeps running normally the whole time. A `sequence` is not a separate thread and does not need to be one. It simply remembers where it paused and picks back up on the right frame.

Notice where the drawing happens. Anything drawn is gone by the next frame, so a sequence changes what is shown, here a `caption` variable, and the `during` block draws it every frame. A `screen.text` inside the sequence itself would show for exactly one frame.

`wait()` also accepts a signal instead of a frame count, pausing until that signal actually fires rather than guessing how many frames it will take:

```tiberscript
sequence do
  trigger("spawnBoss", null)
  wait(boss.introFinished)
  caption = "Fight!"
end
```

This is the same `wait` you already know. It just reads what you pass it: a number waits N frames, `seconds(n)` waits that long, a signal waits for that signal, and a JavaScript promise waits until it settles (19.19). A sequence started inside a state's `during` or `entering` block is cancelled when the game leaves that state, so a cutscene can never keep running into the next level by accident.

For something that has no signal of its own, `wait until` waits for a condition:

```tiberscript
init() = function
  caption = "Reach the door"
  sequence do
    wait until player.x > 200
    caption = "The door opens"
    wait(seconds(1))
    caption = ""
  end
end
```

The condition is checked once a tick, and the sequence goes on during the tick it first is true. It is for a position, a score, a count of the game's own, anything a game would otherwise poll by hand. `wait until` belongs in a sequence; anywhere else it stops with a message that says so.

---

### Transitions: From One Screen to the Next

A game that jumps from its title straight into a level, or from a death straight to a new try, feels broken even when everything in it works. `transition` is the moment in between. It covers the screen, runs a block of your code once the screen is fully covered, and uncovers it. What the block changes (the state, the level, where the hero stands) is never seen changing.

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

`transition.fade(0.4)` takes 0.4 seconds to cover the screen and 0.4 to uncover it. The engine draws it over everything the game draws, the HUD too, so `draw()` needs nothing for it. There are four of them:

| Transition | What it looks like |
| --- | --- |
| `transition.fade(seconds)` | the whole screen goes to a color and comes back |
| `transition.wipe(seconds, from)` | a curtain is drawn across from `"left"`, `"right"`, `"top"` or `"bottom"`, and drawn back |
| `transition.iris(seconds, x, y)` | a circle closes on a point of the screen and opens again from it: on the hero, a door, the place something happened |
| `transition.blinds(seconds)` | strips close like blinds, and open |

Each takes the same options by name:

| Option | What it sets | When left out |
| --- | --- | --- |
| `color` | the color the screen is covered with | `"#000000"` |
| `back` | how many seconds the way back takes | the same as the way out |
| `hold` | how many seconds the screen stays covered before it opens | 0 |
| `from` | the side a wipe or the blinds start from | `"left"` for a wipe, `"top"` for blinds |
| `count` | how many strips the blinds have | 8 |

```tiberscript
init() = function
  level = 1
  hero = data
    x = 0
    y = 0
  end
end

nextLevel() = function
  transition.iris(0.5, hero.x, hero.y, color = "#0B0B16", hold = 0.2) do
    level += 1
    hero.x = -100
  end
end

update() = function
  if key("n").hit and not transition.active then
    nextLevel()
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

The circle closes on the hero, the level changes in the dark, and after a fifth of a second it opens on the new one. The hero stands still while it closes, because the game asks what the transition is doing:

| What | What it says |
| --- | --- |
| `transition.active` | `true` while a transition is running |
| `transition.phase` | `"out"` while the screen is being covered, `"hold"` while it stays covered, `"back"` while it opens, and `""` when none is running |
| `transition.covered` | how much of the screen is covered, from 0 to 1 |
| `transition.kind` | `"fade"`, `"wipe"`, `"iris"` or `"blinds"`, and `""` when none is running |
| `transition.stop()` | ends the transition now, wherever it is |

A transition counts in updates, like everything else in a game, so it takes the same time at 60 and at 120 updates a second, and it stops while the game is frozen with `time.freeze()`. A new transition started while one runs takes its place. With 0 seconds on the way out the screen is covered at once, and only the way back is seen: `transition.fade(0, back = 0.5)` is a fade in, and `transition.iris(0, hero.x, hero.y, back = 0.6)` opens a level on its hero. The block is optional: `transition.fade(0.3)` alone is a blink of the screen, for a hit that should feel heavy or a flash of lightning turned around. A game that changes state at the covered moment writes it as above, and a game with `entering` and `leaving` blocks changes its state inside the block and lets those do the rest.

## 6. Communication Between Objects

As a game grows, objects need to react to things happening elsewhere without being tightly wired together. 1.2 gives you two ways to do this, a quick one and a structured one, and which to use depends on the size of what you are building.

### Quick Events

For a one off announcement that a few different systems care about:

```tiberscript
on("enemyDied") do (info)
  score += info.points
  sfx("kill")
end

on("enemyDied") do (info)
  achievements.checkKillCount()
end

enemy = archetype
  die() = function
    trigger("enemyDied", data(points = 100, x = this.x, y = this.y))
    this.vanish()
  end
end
```

Any number of `on()` listeners can respond to the same `trigger()`. The score system and the achievement system both react independently, and `enemy` never needed to know either one existed. `trigger` is a deliberately new word. `emit` already means particles, and this keeps the two unmistakably separate.

### Declared Signals

Global events are easy, but on a big project it becomes hard to know what an archetype actually announces without reading its whole file. A `signal` is declared right on the archetype, so it shows up as part of that archetype's own definition.

```tiberscript
hero = archetype
  signal died()
  signal healthChanged(oldHp, newHp)

  init() = function
    this.hp = 100
  end

  takeDamage(amount) = function
    local old = this.hp
    this.hp -= amount
    this.healthChanged.trigger(old, this.hp)
    if this.hp <= 0 then this.died.trigger() end
  end
end

init() = function
  player = new hero()
  player.died.connect(onPlayerDied)
  player.healthChanged.connect(updateHealthBar)
end

onPlayerDied() = function
  transit(states.gameOver)
end

updateHealthBar(oldHp, newHp) = function
  shownHp = newHp
end
```

A signal only ever comes from the specific object it was declared on, and only the code that explicitly connects to it ever hears about it. Reading `hero`'s definition tells you everything a hero can announce, in one place. `.disconnect(fn)` stops listening. A signal's own `.trigger()` and a global `trigger()` share a name on purpose, since both mean the exact same thing, announce that something happened. The difference is scope, not meaning: one belongs to a single object, the other belongs to the whole game. Use quick events for something small and global. Use a declared signal once an archetype's behavior is something other parts of the game genuinely depend on.

---

## 7. Input System

Keyboard and mouse input all work as in 1.1, with what is below added for keyboards that differ from one computer to the next, and for the mouse.

### Keyboards on Every Computer

```tiberscript
update() = function
  if key("primary").held and key("s").hit then print("saved") end
  if key("F2").hit then print("the map") end
  if system.platform == "mac" then
    hint = "Command S saves"
  else
    hint = "Control S saves"
  end
end
```

A Mac, a Windows laptop, a Chromebook, a Linux desktop, an iPad with a keyboard: each lays out its keys a little differently, and the engine smooths that over for every game, in every language of the editor.

- **The shortcut key.** `key("primary")` is the key a shortcut uses on the computer the game is played on: Command on a Mac, an iPhone and an iPad, Control everywhere else. `key("command")` (or `"cmd"`, `"meta"`) is Command or the Windows key, and `key("option")` is Option, the same key as `"alt"`.
- **F keys where the system takes them.** A Mac gives most F keys to the system unless fn is held, and a Chromebook's top row runs the browser. On every computer, Control, Shift and a number key together is that F key for the game: Control Shift 2 is F2, Control Shift 0 is F10. The engine's own keys work the same way: Control Shift 3 opens the developer overlay like F3, Control Shift 4 the settings screen like F4.
- **No key stuck down.** When the page loses the keyboard (another window, another tab, a shortcut of the system), every key reads as let go, and on a Mac every key held with Command is let go when Command is, since a Mac does not say so on its own.
- **What kind of computer it is.** `system.platform` is `"mac"`, `"windows"`, `"chromeos"`, `"linux"`, `"ios"`, `"android"` or `"other"`, so a game can name its keys the way that keyboard prints them. When the engine itself asks for a key, it does this already: on a Mac or a Chromebook it says "F3 (or Control Shift 3)".

### The Mouse: the Wheel, and the Shortest Click

```tiberscript
init() = function
  listTop = 0
  clicks = 0
end

update() = function
  if mouse.wheel != 0 then listTop += mouse.wheel * 20 end
  if mouse.hit then clicks += 1 end
end
```

`mouse.wheel` is how the wheel turned since the last update: `1` turned down (toward the user), `-1` turned up, `0` not at all, the same as every other language of microStudio reads it. A list, a map or a page of stats scrolls with it.

`mouse.hit` is never missed. A tap on a trackpad, or a quick tap on a phone, can go down and come up again between two updates of the game; that click still counts, once, on the next update, as a click held down longer does. The same is true in every language: microStudio's own `mouse.press` and `touch.press` count it too.

### Controllers

```tiberscript
update() = function
  local pad = gamepad(0)
  if pad.hit("south") then print("jump") end
  local move = pad.axis("leftX") * 3
end
```

Every controller a browser can see works: Xbox (360, One, Series, Elite and Adaptive), PlayStation (DualShock 3, DualShock 4, DualSense and DualSense Edge), Nintendo Switch (the Pro Controller, Joy Con, the Switch Online controllers, the GameCube controller and its adapter), the Steam Controller, the Steam Deck and Steam's own virtual controller, 8BitDo (Pro 2, Ultimate, SN30 Pro, M30, Lite, Zero and the rest), Logitech (F310, F510, F710), PowerA, PDP, HORI, Razer, SteelSeries, Mad Catz, GameSir, Nacon, Thrustmaster, Google Stadia, Amazon Luna, NVIDIA Shield, and any other USB or Bluetooth controller. A cable or Bluetooth makes no difference to your game: it reads them the same way.

`gamepad(n)` is player n's controller, counted from 0, in the order the controllers first pressed a button, up to eight of them. A browser only shows a controller to a page after one of its buttons has been pressed, so a player's first press is what makes their controller appear. A controller that is unplugged, or whose battery runs out, keeps its player number: plugged back in, it is the same player again, even when the browser now lists it somewhere else.

**Buttons by where they are.** Each brand prints different letters on its buttons, and Nintendo puts A where Xbox puts B. So the names that always work are where a button is. A game written for `"south"` jumps with A on an Xbox controller, Cross on a PlayStation one and B on a Switch Pro Controller: the bottom button on all three.

| Name | Where | Xbox | PlayStation | Nintendo |
| --- | --- | --- | --- | --- |
| `south` | the bottom face button | A | Cross | B |
| `east` | the right face button | B | Circle | A |
| `west` | the left face button | X | Square | Y |
| `north` | the top face button | Y | Triangle | X |
| `lb`, `rb` | the shoulder buttons | LB, RB | L1, R1 | L, R |
| `lt`, `rt` | the triggers | LT, RT | L2, R2 | ZL, ZR |
| `select` | the left middle button | View | Share or Create | Minus |
| `start` | the right middle button | Menu | Options | Plus |
| `ls`, `rs` | a stick pressed in | LS, RS | L3, R3 | LS, RS |
| `home` | the logo button | Xbox | PS | Home |
| `extra` | the one more button | Share | the touchpad, pressed | Capture |
| `dpadUp`, `dpadDown`, `dpadLeft`, `dpadRight` | the D pad | | | |
| `up`, `down`, `left`, `right` | the D pad, or the left stick pushed a third of the way | | | |

Each brand's own names work too. `"a"`, `"b"`, `"x"` and `"y"` are the Xbox letters, so `"a"` is the bottom button on every controller, as it already was in microStudio's own `gamepad.A`. `"cross"`, `"circle"`, `"square"`, `"triangle"`, `"l1"`, `"r2"`, `"zl"`, `"zr"`, `"plus"`, `"minus"`, `"options"`, `"share"`, `"view"` and `"menu"` are the names printed on those controllers. A name that is no button is an error that lists the names.

**confirm and cancel.** On Xbox and PlayStation controllers the bottom button means yes. On a Nintendo controller the right one does, and the bottom one means back. `"confirm"` and `"cancel"` follow each controller's own habit, so a menu feels right in every player's hands. An 8BitDo controller that prints Nintendo's letters follows Nintendo's habit, and its prompts show those letters.

**Reading it.** `.held(name)` is a button held down, `.hit(name)` one pressed on this tick and `.released(name)` one let go on this tick, the same pattern `key()` has. `.amount(name)` is how far, from 0 to 1, for a trigger pulled halfway. `.axis("leftX")`, `"leftY"`, `"rightX"` and `"rightY"` go from -1 to 1, with up positive as everywhere in TiberScript, and `"lt"` and `"rt"` from 0 to 1. `.angle("left")` is the direction a stick points, in degrees (0 is right, 90 is up), or `null` while it rests. `.anyHit()` is the first button pressed on this tick, by name, or `null`: a menu that rebinds a button is one line, `bind("jump", pad.button(pad.anyHit()))`.

**Sticks that drift.** An old stick rarely rests at exactly the middle, and a character that creeps along on its own is the first thing a player notices. A stick reads 0 until it moves past its **deadzone**, 0.15 of the way out, and past it the reading is scaled to start from 0 again, so walking slowly still works. `pad.deadzone = 0.25` changes it for one controller, `controllers.deadzone` for every controller.

**A light touch.** A button counts as held from a fifth of the way down (0.2), and a button the controller itself reports as pressed always counts, however lightly it reports it. Once held, it stays held until it is nearly let go (0.12), so a finger resting half way never flickers between the two. A trigger answers `.held("rt")` from a light squeeze, and `.amount("rt")` still says exactly how far it is pulled. A stick counts as `"left"`, `"right"`, `"up"` or `"down"` from a fifth of the way out past its deadzone, and lets go at 0.12 the same way. microStudio's own `gamepad` object (`gamepad.LEFT`, `gamepad.UP`) answers from 0.4 of the way, where it used to wait for halfway. A controller that felt stiff, that had to be pushed hard before anything happened, now answers the way a console does.

**As quick as a key.** A browser tells a page about a key the moment it goes down. It tells a page about a controller only when the page asks, and a game asks once an update, so a press could wait most of a frame to be seen, and a tap that began and ended between two updates was never seen at all. TiberScript asks again every 4 milliseconds between updates and keeps the deepest press it saw, so the next update has every tap, however short. It rests when the game does, and a recording, a replay and a rewind play the same presses with or without it.

What is left after that belongs to the controller. A controller sends its buttons a fixed number of times a second, more on a cable than over Bluetooth on most, and each maker chose its own number: a DualShock 4 and most 8BitDo controllers are among the quickest, and a Switch Pro Controller over Bluetooth is among the slowest, at about 66 times a second. No game can read a press before the controller has sent it.

**What it is.** `.connected` says whether it is plugged in now. `.kind` is `"xbox"`, `"playstation"`, `"nintendo"`, `"steam"` or `"generic"` (`"none"` before any controller has used that number), known from the vendor and product numbers the browser gives, or from the controller's name. `.name` is what it is, `"DualShock 4"` or `"Switch Pro Controller"`, and `.model` is the same in one short word for code that treats one exact controller its own way: `"dualshock4"`, `"dualsense"`, `"switchpro"`. `.player` is its number, `.joycon` is `"L"` or `"R"` for a single Joy Con, `.setUp` is false for a controller the browser does not lay out the standard way and nobody has set up yet, and `.silent` is true for a controller the browser lists but that has sent nothing so far (both below).

### Every controller at once

```tiberscript
init() = function
  bind("jump", "space")
  bind("jump", controllers.button("south"))
end

update() = function
  local joining = controllers.anyHit("start")
  if joining then
    print("player " + (joining.player + 1) + " joined with a " + joining.name)
  end
end
```

`controllers.button(name)` is that button on whichever controller presses it, for `bind()`; `gamepad(1).button("south")` is that button on player 2's controller only. `controllers.count` is how many are connected, `controllers.list` is each of them, and `controllers.first` is the first one or `null`. `controllers.held(name)` and `controllers.hit(name)` ask every controller at once, and `controllers.anyHit(name)` gives back the controller that pressed it (any button, without a name), which is "press Start to join" in one line. `controllers.hint` is what to tell a player while no controller shows yet, and empty once one does.

Two events say when a controller comes and goes:

```tiberscript
init() = function
  on("controllerConnected") do (pad)
    print(pad.name + " is player " + (pad.player + 1))
  end
  on("controllerDisconnected") do (pad)
    print("player " + (pad.player + 1) + " lost their controller")
  end
end
```

Pausing when a controller disconnects in the middle of a fight is a kindness every console game does, and `controllerDisconnected` is where a game does it.

**Which one is in use.** `controllers.inUse` is `true` while a controller was used after the keyboard and the mouse, so a game shows "press A" to a player holding a controller and "press SPACE" to one at the keys, and changes the moment they change. `controllers.active` is the controller used last, for the prompts that match it, and `controllers.rumble(0.6, 0.2)` shakes every controller at once.

```tiberscript
draw() = function
  screen.clear("#101020")
  local prompt = "press SPACE to start"
  if controllers.inUse then prompt = "press " + controllers.active.label("confirm") + " to start" end
  screen.text(prompt, 0, 0, 10, "#FFFFFF")
end
```

### How Long a Button Is Held: Repeats, Taps and Long Presses

```tiberscript
init() = function
  choice = 0
  charge = 0
end

update() = function
  local pad = gamepad(0)
  if pad.repeat("down") or key("arrowDown").repeat then choice = (choice + 1) % 5 end
  if pad.repeat("up") or key("arrowUp").repeat then choice = (choice + 4) % 5 end
  if pad.tapped("west") then print("a quick swing") end
  if pad.longPress("west", seconds(0.5)) then print("a heavy swing") end
  if pad.doubleTap("south") then print("a second jump") end
  charge = pad.heldFor("east")
end
```

A menu that moves one line for each press makes a player press again and again to cross it. `.repeat(name)` is true on the tick the button goes down, true again after 24 ticks, and then every 6 ticks for as long as it stays down, the way a key held in a text editor repeats. A list scrolls, a number climbs, a cursor crosses a map, all from one held direction. `.repeat(name, delay, rate)` takes a delay and a rate of the game's own, in ticks: `pad.repeat("right", seconds(0.3), 3)`.

It works for anything that is held or not: a button, a trigger, the D pad, and a stick leaned one way (`"up"`, `"down"`, `"left"` and `"right"` are the D pad or the left stick).

| What | What it says |
| --- | --- |
| `pad.heldFor(name)` | how many ticks the button has been held: 1 on the tick it goes down, 0 while it is not held |
| `pad.repeat(name, delay = 24, rate = 6)` | `true` on the first tick of a hold, again after `delay` ticks, then every `rate` ticks |
| `pad.tapped(name, within = 15)` | `true` on the tick the button is let go, when it was held for `within` ticks at most: a tap, not a hold |
| `pad.longPress(name, ticks = 30)` | `true` once, on the tick the hold reaches that many ticks |
| `pad.doubleTap(name, within = 18)` | `true` on a press that comes `within` ticks at most after the press before it |
| `pad.released(name)` | `true` on the tick the button is let go |

A quick swing and a heavy one on the same button is `tapped()` and `longPress()` side by side, as above. A charge that grows while a button is held reads `heldFor()` every tick, and fires on `released()`.

`controllers` answers every one of them for whichever controller it is true of: `controllers.repeat("down")`, `controllers.tapped("south")`, `controllers.longPress("start", seconds(1))`, `controllers.doubleTap("east")`, `controllers.released("rt")`, and `controllers.heldFor("down")` for the longest hold.

**Keys and actions hold the same way.** `key(name).heldFor` counts the ticks a key has been held, `key(name).repeat` is true on its first tick, after 24, then every 6, and `key(name).released` is true on the tick it is let go. `action(name).repeat` does the same for whatever is bound to the action, keys and buttons alike. For a delay and a rate of the game's own, `repeats(ticksHeld, delay, rate)` answers for anything that says how long it has been held:

```tiberscript
init() = function
  bind("next", "arrowRight")
  bind("next", controllers.button("right"))
  page = 0
  volume = 5
end

update() = function
  if action("next").repeat then page += 1 end
  if repeats(key("arrowUp").heldFor, 12, 2) then volume = min(10, volume + 1) end
  if repeats(action("next").heldFor, 40, 1) then page += 10 end
end
```

The engine's own menus repeat as well: a held arrow or a held stick keeps moving through the choices of a `dialogue` and from one `ui.button` to the next.

### How Far Down Counts: Thresholds, Curves, and a Stick as It Is

```tiberscript
init() = function
  controllers.threshold = 0.3
  controllers.stickThreshold = 0.35
  controllers.curve = 1.5
  gamepad(0).setThreshold("rt", 0.8)
  speed = 0
end

update() = function
  local pad = gamepad(0)
  if pad.held("rt") then print("fire") end
  speed = controllers.amount("right") - controllers.amount("left")
  local aim = pad.stick("right")
  local asRead = pad.raw("leftX")
end
```

A trigger is not a switch. It reports how far it is pulled, from 0 to 1, and so does every button of a controller that measures its buttons. Where on the way down a button starts to count is a choice, and it changes how a game feels: a light press makes a jump quick, a firm one keeps a finger resting on a trigger from firing.

| What | What it sets |
| --- | --- |
| `controllers.threshold` | how far down a button or a trigger counts as pressed, on every controller: 0.2 unless it is set. Once held, a button lets go at six tenths of that |
| `pad.threshold` | the same for one controller, which comes before the one for all. `null` gives it back |
| `pad.setThreshold(name, press, release)` | one button of one controller: `pad.setThreshold("rt", 0.8)` asks for a firm pull of the right trigger. `release` is where it lets go, six tenths of `press` unless it is given. `pad.setThreshold("rt", null)` gives the usual back |
| `controllers.stickThreshold`, `pad.stickThreshold` | how far a stick leans, past its deadzone, before it counts as a direction: 0.2 unless it is set |
| `controllers.curve`, `pad.curve` | 1 is a stick as it is. Above 1 the middle of the stick is finer and its end is where it was: 1.5 for aiming, 2 for a slow careful cursor. Under 1 the middle is quicker |
| `controllers.deadzone`, `pad.deadzone` | how far a stick moves before it reads anything at all: 0.15 unless it is set |

**Reading how far.** `pad.amount(name)` is how far, from 0 to 1: a trigger's pull, and for a direction how far the left stick leans that way (the D pad counts as all the way). `controllers.amount("right") - controllers.amount("left")` is a walk that is slow when the stick leans a little and fast when it leans all the way, on whichever controller is in use. `controllers.axis("leftX")` is the stick of whichever controller leans the farthest, and `pad.stick("left")` and `controllers.stick("right")` give a stick as a vector, `x` to the right and `y` up.

**Each stick is four directions.** `"lsUp"`, `"lsDown"`, `"lsLeft"` and `"lsRight"` are the left stick alone, without the D pad, and `"rsUp"`, `"rsDown"`, `"rsLeft"` and `"rsRight"` the right stick. They are names like any button's: `pad.hit("rsLeft")` is a flick of the right stick, `pad.repeat("rsDown")` scrolls with it, `pad.amount("rsUp")` is how far it leans up.

**A stick as it is.** `pad.raw("leftX")`, `"leftY"`, `"rightX"` and `"rightY"` read a stick before its deadzone and its curve, to four places. It is what a calibration screen shows, and what tells a stick that drifts (it rests at 0.04, say) from one that does not.

A player can set the threshold, the deadzone and the curve themselves, on the settings screen every game has (Section 21). What a game sets in its `init()` is where that screen starts.

### Feel in Words, and Letting the Engine Measure

```tiberscript
init() = function
  controllers.feel = "fast"
  controllers.threshold = "light"
  controllers.deadzone = "auto"
  controllers.curve = "aim"
  gamepad(0).setThreshold("rt", "firm")
end

update() = function
  local d = gamepad(0).drift
  watch("left stick rests at", d.left)
end
```

Every one of these settings takes a word as well as a number, and a misspelled word is an error that lists the words there are.

| Setting | Its words |
| --- | --- |
| `controllers.threshold`, `pad.threshold`, `pad.setThreshold(name, ...)` | `"instant"` 0.05, `"light"` 0.1, `"normal"` 0.2, `"firm"` 0.4, `"hard"` 0.6, and `"auto"` |
| `controllers.stickThreshold`, `pad.stickThreshold` | the same words, without `"auto"` |
| `controllers.deadzone`, `pad.deadzone` | `"none"` 0, `"tiny"` 0.05, `"small"` 0.1, `"normal"` 0.15, `"wide"` 0.25, `"widest"` 0.35, and `"auto"` |
| `controllers.curve`, `pad.curve` | `"quick"` 0.7, `"straight"` (or `"linear"`) 1, `"fine"` (or `"aim"`) 1.5, `"finer"` (or `"precise"`) 2 |

**`"auto"` measures the controller in the player's hands.** For a threshold, the engine asks for a careful 0.3 while it learns the controller, for its first half second, and then counts a button as pressed just past where the highest of its buttons sits when nobody touches them: a controller whose buttons all read 0 at rest answers the lightest touch, and a worn one that rests at 0.08 does not press itself. For a deadzone, it watches each stick while it is still and sets the deadzone just past where that stick rests, so a stick that drifts stops creeping and a stick that does not keeps every bit of its travel. It also notes how far each stick has ever reached: a worn stick that never quite gets to the edge still reads 1 at the farthest it goes.

**`pad.drift`** is what `"auto"` has measured: `left` and `right` (where each stick rests when nobody touches it), `leftReach` and `rightReach` (the farthest each has gone), `press` (where a button counts as pressed now), and `leftRest` and `rightRest` (the deadzone each stick has now). It is what a calibration screen shows, and a game can show it in its own options.

**`controllers.feel`** sets the threshold, the stick threshold, the deadzone and the curve together, from one word:

| Feel | What it is |
| --- | --- |
| `"fast"` | answers the lightest touch: the threshold and the deadzone are measured (`"auto"`), and a stick counts as a direction early. For action games, where a jump must come the moment a button moves |
| `"normal"` | the engine as it starts: a threshold of 0.2, a deadzone of 0.15, a straight curve |
| `"precise"` | for aiming: a measured deadzone, a fine curve (1.5), and a stick that leans a little farther before it counts as a direction |
| `"relaxed"` | forgives a resting thumb: a firm threshold, a wide deadzone, and a stick that must lean well over before it counts |

Reading `controllers.feel` gives the word for what is set now, or `"custom"` when the settings are none of the four. The settings screen every game has (Section 21) has a CONTROLLER FEEL line with the same four words, and its BUTTON PRESS and STICK REST lines show the words above, `auto` first.

### The Buttons Only Some Controllers Have

```tiberscript
update() = function
  local pad = gamepad(0)
  if pad.hit("touchpad") then print("the map opens") end
  if pad.has("paddle1") and pad.hit("paddle1") then print("a back button") end
  if pad.hit("share") then print("Share, or Create, or the left middle button") end
end
```

| Name | What it is | Also called |
| --- | --- | --- |
| `extra` | the one more button on the front: the touchpad of a PlayStation controller pressed in, Share on an Xbox Series controller, Capture on Nintendo's, the quick access button on Valve's, Star on 8BitDo's | `touchpad`, `capture`, `qam`, `quickAccess`, `star`, `screenshot` |
| `touch` | a finger resting on a touchpad, without pressing it | `touching` |
| `mute` | the microphone button of a DualSense | `mic` |
| `paddle1`, `paddle2` | the left and right buttons on the back: a DualSense Edge, Valve's controllers | `backLeft`, `backRight`, `l4`, `r4`, `gl`, `gr`, `p1`, `p2` |
| `paddle3`, `paddle4` | the second pair: the function buttons of a DualSense Edge, the lower back buttons of Valve's | `fnLeft`, `fnRight`, `l5`, `r5`, `p3`, `p4` |
| `sl`, `sr` | the two small buttons on the rail of a single Joy Con | |
| `chat` | the chat or voice button some controllers have on the front | `gameChat` |
| `cUp`, `cDown`, `cLeft`, `cRight` | the C buttons of a controller made for the Nintendo 64, where a right stick would be | the same as `"rsUp"` to `"rsRight"`: on any controller, the right stick |

`"share"` is the button each maker calls so: the Share button of an Xbox controller, and the left middle button (Share, or Create) of a PlayStation one.

`pad.has(name)` says whether the controller in the player's hands has that button, so a game shows a prompt for the touchpad only to a player who has one. `pad.label("paddle1")` is what the button is called there: `"Back L"` on a DualSense Edge, `"L4"` on a Steam Controller. `controllers.buttons` is the list of every name a controller answers to.

`extra`, `sl`, `sr` and the C buttons come from any browser. The others are sent only in a controller's own reports, so they answer where the controller is read directly (see "Controllers Read Directly" below), and read as not held everywhere else: a game that uses them still plays, and should keep each of them as a second way to do something.

### Button Prompts That Match the Controller

```tiberscript
draw() = function
  screen.clear()
  local pad = controllers.first
  if pad then
    pad.drawButton("south", -40, -60, 12)
    screen.text("jump", -20, -60, 8, "#FFFFFF")
    pad.draw(0, 20, 120)
  end
end
```

`.label(name)` is the text printed on that button of this controller: `"A"`, `"Cross"` or `"B"` for `"south"`, `"ZR"` for `"rt"` on a Nintendo controller. `.drawButton(name, x, y, size)` draws the button itself as it looks: a green A, a red B, a blue X and a yellow Y on Xbox, the cross, circle, square and triangle in their own colors on PlayStation, the letters on Nintendo's grey buttons, pill shaped shoulders, triggers and middle buttons, and a D pad with the arm lit. It lights up while the button is held. `.draw(x, y, width)` draws the whole controller, every held button lit and the sticks leaning where they point: a screen for testing controllers, or a picture for a controls menu.

### Rumble

```tiberscript
hurt(pad) = function
  pad.rumble(0.6, 0.2)
end
```

`.rumble(strength, seconds)` runs both motors, `strength` from 0 to 1 (0.6 and 0.2 seconds when left out). `pad.rumble(1, 0.3, weak = 0.2)` gives the small motor its own strength. `.stopRumble()` stops it early and `.canRumble` says whether this controller, in this browser, can. `rumble()` gives back `true` when it rumbled and `false` when it could not, and the game carries on either way. A controller read directly (below) rumbles on its own motors in every browser. Read by the browser alone, Chrome and Edge rumble Xbox, PlayStation and Switch Pro controllers, and other browsers often cannot. A forecast, a `whatif` and a replayed test never rumble.

### A Controller Laid Out Its Own Way

Browsers lay out the controllers they know in the standard way above. A controller a browser does not know may come through with its buttons in another order: `.setUp` is then false. `controllers.setup(n)` opens a screen the engine draws over the game, with a picture of a controller, that asks for each button in turn (press the bottom face button, pull the left trigger, push the left stick up, and so on, 21 questions). SPACE skips a button the controller does not have, BACKSPACE goes back one, ESC cancels. What it learns is kept in storage under the controller's id, so it is set up for good, and from then on that controller reads the standard way like any other. `controllers.settingUp` is true while the screen is open (the controller's input is held back from the game meanwhile), and `controllers.forget(n)` goes back to the standard layout.

A player never has to wait for a game to offer that screen. In every TiberScript game, **F2** opens it for the controller that needs it most: one the engine is only guessing at, or else the one used last. When a controller the engine has to guess at joins, a line at the top of the screen says so for a few seconds (`The USB Gamepad is new here: press F2 to set its buttons`). `controllers.setupKey = "f9"` moves it to another key, and `controllers.setupKey = null` turns the key and the engine's own lines off, for a game that has its own controls menu.

### Plugged In or Wireless

| Controller | With a cable | Over Bluetooth |
| --- | --- | --- |
| Xbox One and Series | USB on Windows and Linux. A Mac has no driver for an Xbox controller on a cable: there it is Bluetooth only | hold the pair button on top until the logo flashes quickly, then pair it in the computer's Bluetooth settings (Xbox One S and later; earlier Xbox One controllers need Microsoft's wireless adapter) |
| Xbox 360 | the wired one, USB, on Windows and Linux | the wireless one needs Microsoft's receiver |
| DualShock 4 | micro USB | hold Share and PS until the light bar flashes |
| DualSense | USB C | hold Create and PS until the lights flash |
| Switch Pro Controller | USB C: on a cable it sends nothing until it is asked to. Chrome and Edge ask, and so does TiberScript when it reads the controller directly (below) | hold the small sync button on top until the lights run back and forth. It is read directly wherever that is possible (below), and by the browser itself elsewhere |
| Joy Con | | hold the sync button on the rail. Some systems show a left and a right as one controller, and TiberScript makes them one where the system does not |
| 8BitDo | USB | its own pair button; its mode switch picks how it talks: X is an Xbox controller to every browser, S is a Switch Pro Controller, and D is read by the engine itself |
| Steam Controller, Steam Deck | USB | with Steam running, Steam Input shows them to the browser as Xbox controllers |

A browser sees a controller once the computer does, so pairing is done once, in the computer's own settings, and every TiberScript game then reads it. Which controllers a computer and a browser can read is theirs to decide: when a controller is silent, the first thing to try is the other way of connecting it, and the second is another browser. Nintendo's controllers are the one case where TiberScript does more, and reads them itself (below).

### Nintendo Controllers in Every Browser

Chrome and Edge lay out a Switch Pro Controller and a pair of Joy Cons the standard way. Other browsers do not: they hand the page each button in the order the controller itself reports them, with the D pad as one number that turns as it is pressed. TiberScript reads that order too. A Switch Pro Controller, a Joy Con and the controllers Nintendo makes for the Switch Online games read with their buttons in the right places in every browser that hears them, the D pad included. The same is done for the other controllers browsers often hand over as they are: a DualShock 3, a DualShock 4 and a DualSense where a browser has no layout for them, 8BitDo controllers in their D mode, and Logitech's F310, F510 and F710. `pad.layout` says how a controller is being read:

| `pad.layout` | What it means |
| --- | --- |
| `"standard"` | the browser lays it out the standard way |
| `"nintendo"` | the engine reads Nintendo's own order |
| `"known"` | the browser hands it over as it is, and the engine knows this exact controller's own order |
| `"joycon pair"` | a left and a right Joy Con, read as one controller |
| `"learned"` | it was set up with `controllers.setup()` |
| `"guessed"` | the browser does not know it, nobody has set it up, and the engine reads it the standard way as its best guess (`.setUp` is false) |

**One Joy Con, held sideways.** A single Joy Con is a whole small controller when it is held sideways: its four buttons are the face buttons, its stick is the left stick, turned so pushing away from you is up, and SL and SR are the shoulders. `pad.joycon` is `"L"` or `"R"`.

**Two Joy Cons, one controller.** When the browser sees a left and a right Joy Con apart, TiberScript makes them one controller, the way a Switch does with both in the grip: the left one's buttons are the D pad, minus, ZL, L and the left stick, the right one's are the face buttons, plus, ZR, R and the right stick. One `controllerConnected` event says so. `controllers.pairJoycons = false` keeps them as two controllers, for two players with one Joy Con each. A recent Mac can also combine them itself, in System Settings, and then the browser shows one controller from the start.

**Joy Con 2 and the Switch 2 Pro Controller.** TiberScript knows them by name and by number, and reads them like their older brothers. They talk to a computer over Bluetooth Low Energy, which most computers and browsers do not show to a page as a controller yet: with a USB C cable the Switch 2 Pro Controller works wherever the computer sees it, and over Bluetooth it works once the computer itself lists it as a game controller.

**Where a browser lists a controller and cannot read it.** Some systems talk to a Switch Pro Controller and to Joy Cons themselves, in a fuller way than a browser follows. The browser then lists the controller without being able to read what it sends, or does not list it at all. A Pro Controller on a cable says nothing until it is asked in its own way. TiberScript notices: `pad.silent` stays true, and after ten seconds the engine says on screen what to do. The next part is what it does about it.

### Controllers Read Directly

A controller speaks one language whatever carries it: reports of its buttons, its sticks and its triggers, many times a second. A browser reads those reports and hands a page what it understood, at a pace of its own. Some browsers pass a change on within a few thousandths of a second. Some pass changes on only a few times a second, and a player feels that as a controller that answers late, on every game, whatever the game does. And some cannot read a controller's reports at all.

TiberScript reads the reports itself, the moment each one arrives, for the controllers of the three console makers and of Valve: Nintendo's (the Pro Controller, Joy Cons and the Switch Online controllers), Sony's (the DualShock 4, the DualSense and the DualSense Edge), Microsoft's (the Xbox controllers that pair over Bluetooth) and Valve's (the Steam Controller, the first one and the one of 2026, and the Steam Deck's own). A controller read this way is a controller like any other to a game: `gamepad(0)`, the same button names, the same rumble, and the player number it already had. There are two ways the reports reach the game, and the engine uses whichever is there.

**microStudio reads the controller.** On the computer microStudio itself runs on, the editor opens the controller and passes each report to the game as it arrives: the controller bridge. With the bridge on, every controller named above works in every browser on that computer with nothing to pick and nothing to set, the ones a browser cannot read by itself included. When microStudio starts, it prints whether the bridge is on. On a Mac it is on by itself: microStudio builds the small program it needs the first time it starts, with what a Mac already has. On another system it prints the one line that turns it on.

**The browser hands the controller over.** Chrome, Edge and a game exported for the desktop can give a page the reports of a Nintendo, a Sony or a Valve controller. A browser asks the player first, once: with no controller answering yet, the setup key (F2) opens the browser's list of controllers, and the one picked there is opened by itself every time from then on. A game with a menu of its own asks for the list with `controllers.connect()`, and the browser shows it at the player's next key or click (a browser only shows it then). A game exported for the desktop asks nobody: a Nintendo controller plugged in or paired is read at once.

```tiberscript
update() = function
  local p = gamepad(0)
  if p.direct != "" then
    how = "read directly, by " + p.direct
    battery = p.battery
    local m = p.motion
    if m != null then tilt = m.ax end
  end
  pace = p.pace
end

draw() = function
  screen.clear("#101020")
  if controllers.direct == "" then
    screen.text("this browser reads controllers its own way only", 0, 0, 7, "#C8C8DC")
  end
  if pace > 50 then
    screen.text("this controller is read slowly here", 0, -12, 7, "#F0C070")
  end
end
```

| What | What it says |
| --- | --- |
| `controllers.direct` | `"bridge"` when microStudio's bridge is on, `"webhid"` when the browser can hand controllers over, `""` when neither is there |
| `controllers.connect()` | has the browser show its list of controllers at the player's next key or click; `false` where it cannot |
| `pad.direct` | `"bridge"` or `"webhid"` for a controller read from its own reports, `""` for one the browser reads |
| `pad.pace` | how many thousandths of a second pass between two readings of this controller while it is in use, and 0 while that is not known yet. A small number is a controller that answers at once. A number above 50 is a browser passing changes on slowly, and reading directly is the cure |
| `pad.setLight(color)` | gives the light bar of one of Sony's controllers a color of the game's own (`pad.setLight("#FF8800")`), and `pad.setLight(null)` gives the player's color back. `true` when the controller has a light bar and is read directly, `false` otherwise |
| `pad.motion` | the motion sensor, or `null`: `ax`, `ay`, `az` are how it lies, in g, and `gx`, `gy`, `gz` how fast it turns, in degrees a second |
| `pad.battery` | `"full"`, `"medium"`, `"low"`, `"critical"`, `"empty"` or `"charging"`, and `""` when it is not known |
| `pad.touches` | the fingers on the controller's touchpad, a list with one entry for each: `id`, and `x` and `y` from -1 to 1 (left to right, bottom to top). An empty list when no finger is on it, or the controller has no touchpad |
| `pad.setTrigger(name, strength, from)` | makes a trigger of a DualSense push back: `strength` from 0 to 1, starting `from` that far down (0 unless given). `pad.setTrigger("rt", 0)` lets it go free again. `true` when it was set |

A controller in the player's hands can say things back. This one shakes and turns its light bar red for half a second when its player is hit, then gives the player's own color back:

```tiberscript
init() = function
  hurt = 0
end

update() = function
  local p = gamepad(0)
  if p.hit("south") then
    hurt = 30
    p.rumble(0.8, 0.2)
    p.setLight("#FF2020")
  end
  if hurt > 0 then
    hurt -= 1
    if hurt == 0 then
      p.setLight(null)
    end
  end
end

draw() = function
  screen.clear("#101020")
  screen.text("read every " + gamepad(0).pace + " ms", 0, 0, 8, "#C8C8DC")
end
```

`setLight()` and `rumble()` say `false` on a controller that has no light bar or no motor, or that the browser reads alone, and the game goes on: nothing here has to be checked first.

A touchpad is a small screen with no picture. Each finger on it has a place, and the game decides what a place means: a swipe to turn a page, a thumb dragged to aim, the left half and the right half as two more buttons. This one moves a cursor with the first finger, and makes the right trigger of a DualSense stiff while a bow is drawn:

```tiberscript
init() = function
  cursorX = 0
  cursorY = 0
  drawn = false
end

update() = function
  local p = gamepad(0)
  for finger in p.touches do
    if finger.id == p.touches[0].id then
      cursorX = finger.x * 180
      cursorY = finger.y * 90
    end
  end
  if p.hit("west") then
    drawn = true
    p.setTrigger("rt", 0.7, 0.2)
  end
  if drawn and p.released("rt") then
    drawn = false
    p.setTrigger("rt", 0)
  end
end

draw() = function
  screen.clear("#101020")
  screen.circle(cursorX, cursorY, 4, "#FFD750")
end
```

What reading directly adds to what the browser gives by itself:

- **The controller's own pace.** A report reaches the game the moment the controller sends it. Nothing waits for the browser's next look, so the time from a thumb to the game is the controller's own and the frame's, as it is on a console.
- **No press lost.** Every report is read, so a button tapped and let go between two updates still counts once, and a trigger keeps how far it was pulled.
- **Each stick's own calibration.** A Nintendo controller remembers where each of its sticks rests and how far it goes, as measured where it was made, and again by its owner if they calibrated it. TiberScript asks for those numbers and uses them: a stick that rests a little off the middle reads 0, and reaches 1 at its own end.
- **A Pro Controller on a cable.** It is woken the way it asks to be.
- **The motion sensor and the battery,** on Nintendo's controllers, a DualShock 4 and a DualSense, and the motion sensor of Valve's.
- **The touchpad.** Where each finger is on the touchpad of a DualShock 4 or a DualSense, and on the two pads of Valve's controllers, in `pad.touches`; `"touch"` is held while a finger rests there.
- **The buttons a browser does not pass on.** The microphone button of a DualSense, the back and function buttons of a DualSense Edge, the back buttons of Valve's controllers.
- **Triggers that push back,** on a DualSense, with `pad.setTrigger()`.
- **Rumble on the controller's own motors,** on all three makers' controllers.
- **The light that shows which player it is.** The player's lamp on Nintendo's, and the color of the light bar on Sony's: blue, red, green and pink for players one to four.

A left and a right Joy Con read this way are one controller, and one alone is a small controller held sideways, exactly as above. When the browser lists the same controller too, it is counted once, and when the direct reading takes a controller over from the browser, its player keeps their number. A system that shows a stand in of its own for a controller it reads itself is seen through as well: the stand in is not a second player.

What it does not reach: a browser that hands over no reports, on a computer where the bridge is not on. There a game plays with what the browser itself reads, as before, and `controllers.report()` says which of the two it is, in its last line. An Xbox controller on a cable speaks another way than it does over Bluetooth, and stays with the browser.

### When a Controller Does Not Work

```tiberscript
draw() = function
  screen.clear()
  local y = 60
  for line in controllers.report() do
    screen.text(line, 0, y, 5, "#C8C8DC")
    y -= 9
  end
end
```

`controllers.report()` is a line for every controller the browser lists right now, set up or not: its id (its name, vendor and product numbers), the layout the browser gives it, how many buttons and axes it has, which buttons are held and which axes are moved. After those comes a line for each player, saying how the engine reads that controller: `player 1: DualShock 4 (playstation, known)  held south`. Hold a button and watch which number lights up in the first kind of line, and which name in the second. `controllers.rawReport()` is every button and every axis of every controller as the browser gives them, number by number, for a controller nobody has seen before.

What the report shows says what to do:

- **The controller is not in it at all.** The browser does not see it. Press one of its buttons (a browser shows a controller only after that), check that the computer itself lists it, and try another browser.
- **It is there, and its player line ends with `silent so far`.** The browser lists it and hears nothing from it. Where microStudio's bridge is on, a controller of the three console makers is read directly within a moment and the line goes away by itself. In Chrome and Edge, press F2 for a Nintendo controller: the browser's list opens and the controller is read directly from then on. Where the report's last line says `direct reading: not in this browser`, play from microStudio running on the same computer, or in Chrome or Edge. For any other controller it is a mode the computer does not follow: connect it the other way, switch its mode, or use another browser. `controllers.hint` says the same in a sentence a game can show.
- **It is there with buttons in the wrong places.** Press F2, or call `controllers.setup()`: the engine learns its layout for good.
- **It works, and answers late.** Look at the player's line: `read every 15 ms` is a controller read at its own pace, and a number in the hundreds is a browser passing changes on slowly. `pad.pace` is the same number for a game. Reading directly is the cure: play from microStudio running on the same computer, where the bridge reads the controller for every browser.

Controllers are part of the Timeline (Section 19): what every controller held is kept with each tick, so a recording, a replay, a test that replays a run and a rewind all play a controller exactly, even on a machine with none plugged in.

microStudio's own `gamepad` object still works (`gamepad.A`, `gamepad.LEFT`, `gamepad.press.A`), and it now finds a controller in any of the browser's places: it used to miss one when the browser left the first place empty, which Windows often does.

### Action Mapping

Hardcoding `key("arrowRight")` everywhere means there is no way to offer rebindable controls, and no clean way to treat "move right" the same whether it came from a keyboard or a controller. `bind` and `action` separate what the player means from what they physically pressed.

```tiberscript
bind("moveRight", "arrowRight")
bind("moveRight", "d")
bind("jump", "space")
bind("jump", controllers.button("south"))
bind("jump", touch.button("jump", 110, -70, 18))

update() = function
  if action("moveRight").held then player.x += 3 end
  if action("jump").hit then player.vy = 8 end
end
```

Multiple physical inputs can map to the same action. Change the bindings in one place and every check in your game updates with it, including a settings menu that lets players rebind their own controls without a single line of game logic changing. `touch.button(name, x, y, radius)` makes an on screen button that only appears on a touch screen, so the same game plays on a phone with no extra code.

Actions are also what makes Section 19 work: an action's state on every tick is the one thing the Timeline has to remember to replay your game exactly.

### How Far, and Just Before: Analog Actions and Input Buffering

An action knows more than whether it is held. It knows how far, and when.

```tiberscript
init() = function
  bind("left", "arrowLeft")
  bind("left", controllers.button("left"))
  bind("right", "arrowRight")
  bind("right", controllers.button("right"))
  bind("up", "arrowUp")
  bind("up", controllers.button("up"))
  bind("down", "arrowDown")
  bind("down", controllers.button("down"))
  bind("jump", "space")
  bind("jump", controllers.button("south"))
  ship = data(x = 0, y = 0, vy = 0, onGround = true)
end

fixedUpdate() = function
  local steer = actionVector("left", "right", "down", "up")
  ship.x += steer.x * 3
  ship.y += steer.y * 3
  if action("jump").within(8) and ship.onGround then
    action("jump").consume()
    ship.vy = 6
  end
  if action("jump").releasedWithin(1) and ship.vy > 2 then ship.vy = 2 end
end
```

**How far.** A key, a touch button and a mouse button are held all the way or not at all, but a stick and a trigger have every amount in between, and `action(name).amount` is it, from 0 to 1. `actionAxis(negative, positive)` is one axis from two actions, from -1 to 1, and `actionVector(left, right, down, up)` is a direction from four, never longer than 1: push a stick half way and the ship goes at half speed; turn it round a circle and the ship turns round the same circle at an even speed, with no jumps between eight directions. With keys, the same calls give the eight directions, and the diagonals are no faster than straight lines. A stick's direction counts as held from a quarter of the way.

**Just before.** A player presses jump a moment before the hero lands, and expects a jump. `action(name).within(ticks)` is `true` if the action was pressed within the last few ticks and not used yet, so the press is kept until the hero can jump, and `.consume()` uses it up, so one press is one jump. `.heldFor` is how many ticks it has been held, for a charged attack, and `.releasedWithin(ticks)` is `true` just after it is let go, for a short hop when the button is let go early, as above.

All of it is part of the action's state on each tick, so a replay, a rewind, a forecast and a game played online all see the same amounts, the same presses and the same times.


---

## 8. Visuals and Audio (Scoped Effects)

`screen.clear`, `fillRect`, `fillCircle`, `fillRound`, `fillTriangle`, `line`, `sprite`, the rest of microStudio's screen, `view`, and `alter` all work exactly as in 1.1.

`emit()`, `flash()`, and `shake()` show whether your game draws in `update()` or in a separate `draw()`, and particles emitted into a world drawn inside a `view` block move with that view, both since 1.1's last update. `emit()` knows these looks by name: `fire`, `smoke`, `spark`, `sparkle`, `dust`, `boom`, `hit`, `hurt`, `slash`, `spawn`, `levelup`, `dash`, `coin`, `heal`, `ice`, `lava`, `void`, `blood`, `slime`, `holy`, `snow`, `magic`. Any other name gives white sparks, as before.

### A Sprite's Own Size and Turn

```tiberscript
init() = function
  angle = 0
end

update() = function
  angle += 2
end

draw() = function
  screen.clear()
  screen.drawScale(1.5, 1.5)
  screen.drawRotation(angle)
  screen.sprite("icon", 0, 0, 32)
  screen.drawRotation(0)
  screen.drawScale(1, 1)
end
```

`screen.drawScale(x, y)` stretches the sprites drawn after it around their own anchor (`screen.anchor()`), a negative number mirroring them, and `screen.drawRotation(degrees)` turns them around it: a shooting star scaled and turned where it flies, an arrow flipped upside down. Unlike `screen.scale()` and `screen.rotate()`, which move the whole world, these leave where things are drawn alone. Set them back to 1 and 0 after.

### Sprite Animation

`tween` animates a property smoothly toward a target. It was never meant for cycling through a walk cycle. `.animate()` is.

```tiberscript
player.animate("walk", ["hero_walk1", "hero_walk2", "hero_walk3", "hero_walk4"], 8)
player.animate("idle", ["hero_idle1", "hero_idle2"], 20)

update() = function
  if action("moveRight").held then
    player.playAnimation("walk")
  else
    player.playAnimation("idle")
  end
end
```

The last number is how many frames each animation frame holds for. Switching which animation is playing does not restart the whole system from scratch. It picks up cleanly, so a character does not visibly stutter every time they stop and start moving. `player.flip = true` mirrors the sprite around its own center, which is what facing left should have meant all along.

Any one frame of an animated sprite can be drawn by name: `screen.sprite("cat_run.2", x, y, 16)` draws frame 2 (frames count from 0) and holds it there. A game can then pick the frame itself, from how far a character has walked for example, and play a footstep on exactly the frames where a paw lands. A sprite can be drawn in a box of its own, with a width and a height, `screen.sprite("cat_attack", x, y, 27, 19)`, and a negative width draws it mirrored, so a sprite drawn facing right faces left too.

### Camera

`view x=N, y=N do` still works for a one time offset. For a camera that actually behaves like a camera:

```tiberscript
camera.follow("player", 0.1)
camera.bounds(-500, -300, 500, 300)
camera.layer("background", 0.3)
```

`follow` smoothly eases toward the target each frame using the same lerp `math.lerp` uses internally. `bounds` keeps the camera from ever showing past the level edge. `layer` sets up parallax, so a background can move at 30 percent of camera speed while the foreground moves at full speed, which is what makes a flat 2D scene read as having real depth.

### Draw Layers

Draw order used to just be call order. That breaks down fast once you have a background, characters, effects, and a UI all fighting over the same frame.

```tiberscript
screen.layer(0)
background.draw()

screen.layer(1)
player.draw()
enemy.draw()

screen.layer(2)
ui.healthbar(player.hp, 100, -100, 90, 200, 16)
```

Everything drawn on a higher layer renders on top of everything on a lower one, regardless of the order the draw calls happen in that frame.

### Tween Easing

Linear motion was the only option in 1.1. Real animation almost never wants linear.

```tiberscript
tween("scorePanel", "x", 0, 30)
tween("player", "scale", 2, 20, ease = "bounce")
tween("door", "y", 100, 40, ease = "easeOut")
```

Available easing values are `linear` (the default), `easeIn`, `easeOut`, `easeInOut`, `bounce`, `elastic`, `back`, `anticipate`, `smooth`, `sine` and `expo`, the same eleven curves `ease()` gives as plain numbers (Section 2). A name that is none of them warns once, with the list. A UI panel sliding in with `easeOut` or a collected item popping with `bounce` reads completely differently than the same motion done linearly, at no extra cost to write.

### audio

`sfx()`, `tune()`, `silence()`, `pause()`, `resume()`, `alter volume= pitch=` and `audio.beep()` all work exactly as in 1.1. What 1.1 cannot do is set a volume for a whole channel and leave it there. Now it can.

```tiberscript
audio.setVolume("music", 0.5)
audio.setVolume("sfx", 0.8)

if audio.has("roar") then
  sfx("roar")
else
  audio.beep("volume 16 duration 220 square C2 C3")
end
```

Every `sfx()` and `tune()` call respects its channel's current volume automatically. No changes needed to any existing 1.1 call for this to work. `audio.has(name)` says whether the project has a sound or music track by that name, and `sfx()` of a name that does not exist now warns once, with the line, instead of silently playing nothing.

Sounds and music made in microStudio's Sound Studio and Music Studio are ordinary project sounds, so `sfx("coin")` and `tune("theme")` play them with nothing else to set up.

---

## 9. UI and Dialogue

Screen drawing is still exactly how you would build custom UI by hand. For the things almost every game needs, 1.2 adds built ins so you are not rebuilding a button from four rectangles and a click check every single project.

### UI Primitives

```tiberscript
ui.button("Start Game", 0, 0, 120, 40) = function
  transit(states.game)
end

ui.healthbar(player.hp, player.maxHp, -100, 90, 200, 16)
ui.textbox("A long line of quest text that needs to wrap automatically across several lines.", -100, 0, 200)
```

`ui.button(...) = function` reads the same way `schedule(N) = function` always has: the block is what happens when it is pressed. A button draws itself every frame you call it, and responds to the mouse, a touch, or the action bound to `"confirm"` when it has focus. The arrow keys, Tab and a gamepad's up and down move the focus from button to button; with no `"confirm"` bound, Enter presses the focused one.

### Dialogue

A real branching conversation system. This is separate from generic UI because conversations need to sequence and wait for the player, not just render once and sit there.

```tiberscript
dialogue "Welcome, traveler." speaker="Elder" do
  wait(60)
  dialogue "The village needs your help." speaker="Elder"
  wait(60)
  choice "Accept" do
    trigger("questAccepted", null)
    transit(states.quest)
  end
  choice "Not now" do
    transit(states.game)
  end
end
```

`dialogue` blocks pause the calling context, much like `sequence`, until the player advances or picks a `choice`. A `choice` block runs only if the player actually picked that option, and the whole `dialogue` block ends once one has run.

Choices put straight inside a `dialogue` block show under its text at once. After a `dialogue` line inside the block, like the one above, they show once the player has moved past that line. For a question the player answers right away, ask it in the outer `dialogue` and put the choices directly under it.

`Tiber.talking` is true while a dialogue box is on screen, so the hero can stand still while the player reads:

```tiberscript
fixedUpdate() = function
  if not Tiber.talking then
    applyPhysics("player")
  end
end
```

A dialogue started by the code of one state belongs to that state. When the game leaves the state, the dialogue ends and its box closes with it, so a box is never left on screen with nothing behind it.

### HTML Pages Over the Game

For a screen that is easier to build as a web page (a shop, a menu with many choices, a map with text, a board of scores), a game shows one of its own HTML pages over its screen. A page is an asset of the project, `assets/shop.html`: the Assets tab makes a new one with its HTML page button, and edits it as HTML. It is a whole web page, its own HTML, CSS and JavaScript.

```tiberscript
gold = 120

init() = function
  html.show("shop")
  html.set("shop", "gold", gold)
end

update() = function
  local text = html.receive("shop")
  while text != null do
    if text == "buy sword" and gold >= 50 then
      gold -= 50
      html.set("shop", "gold", gold)
    end
    if text == "close" then html.hide("shop") end
    text = html.receive("shop")
  end
end
```

And the page, `assets/shop.html`:

```
<p>Gold: <b id="gold">0</b></p>
<button data-send="buy sword">Buy a sword</button>
<button data-send="close">Leave</button>
```

| Call | What it does |
| --- | --- |
| `html.show(name)` | shows the page over the game; the page takes the mouse, touch and keys |
| `html.hud(name)` | shows the page over the game; the mouse, touch and keys stay the game's |
| `html.hide(name)` | hides the page, kept as it is for the next time it is shown |
| `html.send(name, text)` | a text for the page |
| `html.receive(name)` | the oldest text the page sent, or `null` when none is waiting |
| `html.set(name, id, text)` | the text of the page's element with that id |
| `html.page(name, text)` | a page made from a text instead of a file; given again, the page is made again from it |

In the page, an element with `data-send="..."` sends that text when it is clicked, `game.send(text)` sends one from the page's own JavaScript, and `game.on(function (text) { ... })` gets each text `html.send()` sends. The page lies exactly over the game's screen and its background is see through until it gives itself one. A hidden page keeps running, so it can be made ready with `html.set()` and `html.send()` before it is shown, and a path in it (`sprites/coin.png`) is the project's own file. What a page writes with `console.log`, and its errors, appear in the console with the page's name.

**A page written in the game's own code.** A page does not have to be a file. The simplest way of all is an `html` block, a page written in TiberScript's own file whose values keep themselves up to date (Section 19, Part Five). `html.page(name, text)` makes one from a text, and a `javascript` block (19.19) holds a whole page in one JavaScript template text: its HTML, its CSS and its own JavaScript, with `${}` for the game's values.

```tiberscript
gold = 120

javascript
  html.page("shop", `
    <style>
      .panel { position: absolute; right: 4%; top: 10%; padding: 12px; border-radius: 12px; background: rgba(20, 30, 70, 0.9); color: white }
      button { display: block; width: 100%; margin: 4px 0 }
    </style>
    <div class="panel">
      <p>Gold: <b id="gold">${gold}</b></p>
      <button data-send="buy sword">Buy a sword</button>
      <button data-send="close">Leave</button>
    </div>
    <script>
      document.addEventListener("keydown", (e) => { if (e.key === "Escape") game.send("close") })
    </script>
  `)
end

init() = function
  html.show("shop")
end

update() = function
  local text = html.receive("shop")
  while text != null do
    if text == "close" then html.hide("shop") end
    text = html.receive("shop")
  end
end
```

A page made from a text is shown, hidden, sent to and read exactly like a page from a file. Given again, `html.page()` makes the page again from the new text, and what was sent to it meanwhile waits for it. A text given for a name that also has a file takes the file's place. The page keeps its `<script>`, its styles and its links in full, and a game exported as a web page or a desktop app carries it unchanged.

`html` is the same in every language microStudio runs, so a page made for one game works in another. Inside a `tiberscript 1.1` or `1.0` block, `html` is the game's own name, as `time` and `camera` are.

---

## 10. Dynamic Objects

`register()` and auto creating any object by name both work exactly as in 1.1. One addition for when you need a lot of similar objects at once.

```tiberscript
repeat(5) do
  local id = "coin" + coinCount
  register(id, random(-150, 150), random(-80, 80))
  coinCount += 1
end
```

`register()` already worked inside a loop in 1.1. What is new is `array.ofObjects(tag)`, which collects every currently registered object carrying a given tag into a real array you can iterate, sort, or filter with the standard library above, instead of tracking a separate list by hand alongside the tag system.

```tiberscript
coins = array.ofObjects("currency")
closest = array.sort(coins, byDistanceToPlayer)
if closest.length > 0 then
  arrow.pointAt(closest[0])
end
```

---

## 11. Physics and Logic (Tags and Collision)

`gravity()`, `applyPhysics()`, `bounce()`, `mark()`, `collide()`, `near()`, and `.vanish()` all work exactly as in 1.1.

### Fixed Timestep Physics

`update()` runs once per rendered frame, and frame rate can vary depending on the device. That is fine for input and drawing, but physics run at an inconsistent rate can behave differently on a fast machine than a slow one, which is a real bug, not a cosmetic one. `fixedUpdate()` runs at a steady rate no matter the frame rate, the same separation Unity, Godot, and most serious game engines use for exactly this reason.

```tiberscript
fixedUpdate() = function
  applyPhysics("player")
  applyPhysics("boulder")
end

update() = function
  if action("jump").hit then player.vy = 8 end
  view x=player.x, y=player.y do
    player.draw()
  end
end
```

Put anything that needs consistent, predictable timing in `fixedUpdate()`. Input reading and drawing stay in `update()`, which still runs every rendered frame the same as always. Using only `update()`, the way 1.1 always worked, is still completely valid for anything that does not need this. It is also the line Section 19 is drawn on: everything that happens in `fixedUpdate()` can be rewound, forecast, and replayed.

### Mass and Friction

1.1's physics treated every object the same. A boulder and a coin fell at the same rate and slid forever with nothing to slow them down.

```tiberscript
boulder.mass = 5
boulder.friction = 0.2
applyPhysics("boulder")
```

Heavier objects respond to force more slowly. Friction gradually reduces velocity each frame it is applied. Leave either one unset on an object and it behaves exactly like 1.1 did, with no migration needed for existing projects.

### Real Collision Shapes

1.1's `collide()` checked boxes. Not everything is a box.

```tiberscript
this.collisionShape("circle", 16)
this.collisionShape("box", 32, 48)
```

### Layers and Resolution

```tiberscript
bullet.collisionLayer("playerBullet")
bullet.collidesWith("enemy")

local hit = collide("enemy")
if hit then
  resolve(this, hit)
end
```

`collidesWith` means a collision check only ever runs against the layers you actually care about. That is cheaper, and it eliminates a whole category of bugs where two things that should never interact end up colliding by accident, like a player's own bullets hitting each other. `resolve()` pushes the two objects apart instead of just reporting that they overlap, which `collide()` alone has always done. When both have a `mass`, the lighter one moves further.

### Tile Maps

1.1 can already draw a microStudio map with `screen.map()`, read a tile with `maps["level1"].get(x, y)`, and build one with `new Map(w, h, tileW, tileH)`. What every platformer then writes by hand is the collision: stand on the tiles, stop at the walls. 1.2 does it for you:

```tiberscript
level = tilemap("level1", x = -200, y = -120)
level.solid("ground", "wall", "crate")
level.oneWay("ledge")

fixedUpdate() = function
  applyPhysics("player")
  level.collide(player)
  if player.onFloor and action("jump").hit then player.vy = 8 end
end
```

`tilemap()` wraps a project map at a place in the world. `solid()` names the tiles that block, `oneWay()` the ones you can jump up through and land on. `level.collide(obj)` moves an object out of every solid tile it went into, along the shorter way, and sets `obj.onFloor`, `obj.onWall` and `obj.onCeiling` for that tick, which is the whole of platformer collision. `level.tileAt(x, y)` reads the tile at a world position, and `level.set(x, y, tile)` changes one, so a block can break or a door can open.

---

## 12. Debugging and Error Handling

`attempt`, `recover`, `debug()`, and `Tiber.errors` all work exactly as in 1.1, with one difference: an error that `recover` catches is no longer printed in the console. It is the program working as written, which is how every other language treats an error it catches. It is still in `Tiber.lastError`, and in `Tiber.errors` with the kind `"handled"`.

### Better Error Messages

1.1's errors were accurate but short. `Unexpected token 'x' in expression` tells you something broke. It does not tell you what TiberScript expected instead. 1.2's messages say both: what was found, and what would have worked there instead, the same way a helpful person pointing out a mistake would.

```text
Line 42: 'states.mnue' is not one of the states: menu, game, paused, gameOver.
         Did you mean 'states.menu'?
Line 57: This 'if' has no 'then'. An if reads: if <condition> then
```

### Mistakes Found as the Game Loads

When a game starts in the editor, all of its files are read once more, together, for the mistakes that would otherwise wait for their line to run, or never say anything at all. Each one is a warning in the console, with its file and line, said once. The game still runs, as it would have.

```text
In main, line 12: 'scroe' is read but never set anywhere in the game: did you mean score?
In main, line 14: There is no function 'jmup': did you mean jump()?
In main, line 15: jump() takes 1 value, and is given 2 here: the last one is lost
In main, line 16: 'livs' is only ever changed (+=, -=...), never set: did you mean lives?
In hero, line 8: 'hp' is never set: inside a method, the object's own hp is this.hp
In main, line 18: screen has no 'fillrect': did you mean screen.fillRect()?
In main, line 19: sfx(): the project has no sound 'jmp': did you mean 'jump'?
In main, line 20: screen.sprite(): the project has no sprite 'heroo': did you mean 'hero'?
In helpers, line 3: TiberScript has a built in lerp(), and a call of lerp() is always the built in's: this lerp() of the game is never called. Give it a name of its own
```

A name set in any file counts for every file, and so does every name a `javascript` block uses or a text names (`register("boss")`, `Tiber.addCommand("tripled")`). A name read inside an `attempt` is left alone: that is code that expects a name may not be there. A name read nowhere near any name the game sets is not said either: a value that starts as `null` on purpose is a game's own business. An exported game is not checked.

The running game's own messages say more, too:

```text
Line 3: Cannot call 'player.move()': player is null
Line 7: Unknown method '.mvoe()': did you mean .move()?
Line 9: Unknown command 'jmup': did you mean jump()?
Line 9: 'speed' is 5, not a function: it cannot be called
Line 4: 'Heor' is not an archetype: did you mean Hero?
Line 2: Too many calls inside one another: a function calls itself (or two functions call each other) with nothing to stop it
```

### Loops That Run Too Long

1.1 stopped any `for` loop at 200,000 steps and any `while` loop at 50,000, even when the loop was doing exactly what you meant, like filling a big grid. 1.2 has no step limits. It has a clock: a single loop that keeps the game from drawing a frame for two whole seconds is stopped, and the error names the loop, its line, and how many times it ran. A loop that is merely big never trips it.

### Testing

```tiberscript
test "heal never exceeds max hp" do
  hp = 90
  heal(25)
  assert(hp == 100)
end

test "collide returns null when nothing is hit" do
  assert(collide("nonexistent_tag") == null)
end
```

`test` blocks do not run during normal play. They run on demand, so you can build confidence in the systems underneath your full length game as it grows, instead of only finding out something broke by playing through the whole thing again after every change. A failed `assert` reports the line and the values on both sides of the comparison, not only that it failed. Section 19 adds tests that play your game.

On demand means `Tiber.runTests()`: type it in the console while the game runs, or call it from your own code. Each test runs on the world as it is and the world is put back afterwards, so a test can change anything. It prints a line per test and gives back a report (`passed`, `failed`, and each result with its line and message); `Tiber.runTests("heal")` runs only the tests with "heal" in their name.

---

### watch(): Values on the Screen

`print()` writes a line to the console every time it is called, and a value printed sixty times a second is a wall of text nobody can read. `watch(label, value)` shows the value on the game's own screen, in a corner, on one line that changes in place.

```tiberscript
init() = function
  hero = data
    x = 0
    vx = 2
  end
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

Each label has its own line, in the order the labels first appeared. A number is shown to three places, a list or an object the way `inspect()` writes it, and a label given with no value is a line by itself: a flag that says "the game is in this branch right now". A line whose `watch()` has not been called for two seconds goes away, so `going left` above is on the screen only while the hero goes left.

`watch()` costs next to nothing and changes nothing in the game, so it can sit in the busiest code. `Tiber.watch = false` hides every line without taking a single call out, which is how a game is shown to someone, and `Tiber.watch = true` brings them back. A forecast, a `whatif` and a replayed test show nothing.

When to use which: `watch()` for a value that changes all the time, `print()` for something that happens once, the developer overlay (F3) for how fast the game runs, and `time.debug()` (Section 19) for going back to the tick something went wrong.

## 13. Extensibility: The Tiber Namespace

Every way to reach into TiberScript itself lives under `Tiber`. It is the one namespace in this whole handbook that stays capitalized, because it is not a generic utility like `math` or `array`. It is the language's own name.

```text
Tiber.addCommand(name, fn)
Tiber.addScript(name, code)
Tiber.extendArchetype(archetypeName, methodName, fn)
Tiber.errors
Tiber.version
Tiber.hasFeature(name)
Tiber.showDebugOverlay(on)
Tiber.runTests(filter)
Tiber.tunables()
Tiber.setTunable(name, value)
Tiber.forge
Tiber.resolution
Tiber.shaderScale
```

`Tiber.addCommand()` is unchanged from 1.1: register a brand new command, usable anywhere in your game, checked before any built in so it can even override one. `Tiber.addScript()` is unchanged from 1.1: register a named sub script, run later with `runScript()`. `Tiber.errors`, `Tiber.lastError` and `Tiber.version` are unchanged from 1.1: every logged error and warning, what went wrong in the last `attempt`, and the running version as a string, now `"1.2"`. Inside a `tiberscript 1.1 ... end` block `Tiber.version` is `"1.1"`, and on a line of its own at the top of a file it marks the whole file as that version (Section 1). `Tiber.forge` is the Forge itself, what it made of each function and the say over it (19.25), and `Tiber.resolution` and `Tiber.shaderScale` are how finely the game and its shaders are drawn (21.3 and 19.37).

`Tiber.extendArchetype()` is new in 1.2. It attaches or overrides a method on an archetype from outside the file that archetype was defined in, without editing that file at all. The archetype can be given by name, or as the archetype itself, which is the way to reach one from an imported module, `enemies.slime`. Inside the new method, `super.name()` calls the method it replaced, if there was one:

```tiberscript
Tiber.extendArchetype("enemy", "onHit", function(damage)
  screen.text("-$(damage)", this.x, this.y + 20, 12, "red")
end)
```

Useful for a plugin style addon, or for keeping a shared library archetype untouched while still customizing it per project.

`Tiber.hasFeature(name)` is new in 1.2. It checks whether a specific capability exists before using it, which a version number alone cannot tell you once engines start adding features between versions:

```tiberscript
if Tiber.hasFeature("timeline") then
  time.keep(seconds(10))
end
```

`Tiber.version` and `Tiber.hasFeature` exist for the same reason: a script meant to be shared or reused should be able to check what engine it is actually running on, rather than assuming and breaking outright. The names `hasFeature` knows are the section names of this handbook in lowercase: `modules`, `traits`, `enums`, `sequences`, `signals`, `gamepad`, `controllers`, `timeline`, `swarms`, `forge`, `javascript blocks`, `version blocks`, `buffers`, `wasm`, `shader lists`, `shader quality`, `resolution`, and so on. It is how a 1.2 script stays safe on the engines that come after 1.2. It cannot guard new syntax on an older engine, since an engine that does not understand `import` cannot read the file far enough to ask. What came later has names of its own too: `"time speed"`, `"slow motion"`, `"freeze"`, `"time.ago"`, `"time.forward"`, `"mirrors"`, `"water"`, `"reflections"`, `"forge profile"`, `"forge cache"`, `"shared memory"`, `"transitions"`, `"colors"`, `"springs"`, `"watch"` and `"motion"`.

`Tiber.showDebugOverlay(true)` draws a live panel over your game showing every `trigger()` as it fires, which tags the object under your mouse cursor currently carries, and where the frame's time went, function by function, so a slow frame points at the line that made it slow. Useful while chasing down exactly why two objects are not colliding the way you expect, without adding your own `debug()` calls everywhere and then having to remember to remove them. With the Timeline on, the overlay also has a scrubber along the bottom: drag it back to watch the last seconds of your game again, frame by frame. The game holds on the tick you leave it at; click an object to see its fields as they were then, and the green arrow at the bar's end plays on from there.

---

### Less Motion for a Player Who Asks: Tiber.motion

A screen that shakes and flashes is part of how a hit feels. For some players it is also what makes a game unplayable: motion on a screen can bring on nausea, and strong flashes are worse than that. Systems have a setting for it, and a game should have one too. `Tiber.motion` is that setting, already wired to everything.

```tiberscript
init() = function
  if Tiber.prefersLessMotion then
    Tiber.motion = 0.3
  end
end

hit() = function
  shake(6, 12)
  flash("#FFFFFF", 0.6, 8)
end

update() = function
  if key("h").hit then
    hit()
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

| What | What it is |
| --- | --- |
| `Tiber.motion` | how much the screen may shake and flash, from 0 (not at all) to 1 (as written). It starts at 1. Every `shake()` and every `flash()` is scaled by it from then on |
| `Tiber.prefersLessMotion` | `true` when the player's own system is set to reduce motion, `false` when it is not or cannot be asked |

The game's code does not change: `hit()` above calls `shake(6, 12)` whatever the setting is, and the engine makes it a shake of 1.8 at 0.3 and no shake at all at 0. Put the setting in the options menu next to the volume, start it from `Tiber.prefersLessMotion`, and keep it with `save.set()` (Section 15). What `Tiber.motion` does not touch is what the game draws itself: a background that scrolls fast or a camera that swings is the game's own to calm down, by reading `Tiber.motion` where it matters.

## 14. Maintainability (Sub Scripts)

`Tiber.addScript()` and `runScript()` are unchanged and still fully supported. See the note in Section 3 on how this relates to the module system.

---

## 15. Saving Data

The `access "slot" as var do ... end` block is unchanged and still works exactly as in 1.1. For anything beyond "does this save slot exist," 1.2 adds a structured system on top.

```tiberscript
save.set("highScore", 5000)
save.set("unlockedLevels", [1, 2, 3])

best = save.get("highScore", 0)
save.write()
```

`save.get` takes a default value, which comes back if the key was never set. `access` blocks are still there for full manual control over a whole save file. `save.set` and `save.get` are for the common case: one value, one default, no ceremony.

For games with more than one save file:

```tiberscript
slots = save.list()
save.use("slot2")
save.delete("slot3")
```

Saving one value at a time works well for simple state. For a whole archetype instance, `save.setObject` and `save.getObject` save and restore every field at once instead of copying them out one by one:

```tiberscript
save.setObject("player", player)
save.write()

player = save.getObject("player", new hero("Adventurer"))
```

The second argument to `save.getObject` is what you get back if that save never happened, the same default pattern `save.get` and `table.get` already use.

### Where a Save Is Kept, and That It Is Kept

A save is kept in the browser's storage, under the address the game is played at. Four things about it are worth knowing, and none of them needs a line of code.

- **A save is always written.** The browser's storage is shared by everything on the site, the compiled code the Forge keeps among it (Section 19, Part Three). When it is full, the compiled code gives way and the save is written.
- **A save that cannot be written is said.** A browser set to keep nothing, or one whose storage is switched off, refuses a save. The game keeps running with what it saved, the console says once that the save could not be written, and it is written as soon as the browser takes it.
- **Two pages of one game share their saves.** The game in the editor's run window and the same game in a tab of its own read and write the same saves, and neither writes its older copy over what the other just saved.
- **A renamed game keeps its saves.** A project given a new name has a new address. Its saves are found under the older one and carried over at the first save.


### A Whole Game in One Line

`save.set` keeps the values you choose. `save.game` keeps everything:

```tiberscript
init() = function
  bind("quickSave", "k")
  bind("quickLoad", "l")
end

fixedUpdate() = function
  if action("quickSave").hit then save.game("quick") end
  if action("quickLoad").hit then save.loadGame("quick") end
end
```

`save.game(slot)` writes down the whole game as it is on this tick: every global and every name a file keeps for itself, every object and list they reach (an archetype instance by the name of its archetype, so it comes back with its methods), every swarm and every member in it, the state, the tick, gravity, and the random numbers. `save.loadGame(slot)` puts all of it back and answers `true`, or answers `false` when nothing was saved there. Leave the slot out and it is called `"game"`.

Loading puts every value back into the objects the running game already has, wherever they match, and makes the rest. So whatever was connected to them stays connected: a signal's listeners, a camera following the hero, a list another object holds. Functions and signals stay as the running game has them. And because the random numbers come back too, the game plays on from a load exactly as it did from the save.

**One object stays one object.** Two names for the same object are still two names for one object after a load. A game that keeps the room the hero stands in as `room`, while that same room is also one of a list of every room, gets back the right room, and every room in the list keeps its own values, whichever room `room` pointed at the moment the game was loaded.

Three things are not part of a saved game. `timeless` values, which are kept outside the world on purpose (a high score saved with `save.set` stays the best one ever reached). Tweens and timers that are running. And sequences and dialogue partway through.

```tiberscript
if save.hasGame("quick") then
  local info = save.gameInfo("quick")
  print("saved at tick " + info.tick + " in state " + info.state)
end
save.deleteGame("quick")
```

`save.gameInfo(slot)` tells when a game was saved: `tick`, `savedAt` (the time on the wall, in milliseconds) and `state`. Measured on a platform game of six thousand lines in the middle of a fight, with 291 objects and lists: the save is 41 KB, loading gives back the world exactly as it was saved, and three hundred frames played after the load came out the same as the three hundred played after the save, frame for frame.

---

## 16. Networking and Multiplayer

microStudio has real multiplayer support built into the engine. 1.2 is the first version of TiberScript that can reach it.

```tiberscript
network.on("playerJoined") do (who)
  print(who.name + " joined the game")
  register("player_" + who.id, 0, 0)
end

network.send("position", data(x = player.x, y = player.y))

network.on("position") do (msg, fromPlayer)
  local remote = table.get(otherPlayers, fromPlayer.id, null)
  if remote then
    remote.x = msg.x
    remote.y = msg.y
  end
end
```

Messages from other players arrive through `network.on`, not the plain `on` of Section 6. The two are kept apart on purpose: a local `trigger("position", ...)` in one file and a network message called `"position"` from another player are different things, and one should never be mistaken for the other.

To play it: turn on the project's networking option (in its settings), press **Start** on the run window's Server bar, and open the game in two windows. You write no server code: a game with no `serverUpdate()` of its own gets a server built in, which passes every message to every other player, from the one who sent it, and announces `"playerJoined"` and `"playerLeft"`. `network.id` is this player, `network.players` everyone connected, and `network.connected` whether this game is connected at all.

Sending positions by hand works, and it is how most games start. For action games there is something much better in Section 19: rollback, where you send nothing by hand at all, and `online`, the one word that adds everything else playing online needs to it, a room, a lobby, a link and a QR code to join, players who watch and players who leave and come back (19.7). With `online` you do not even press **Start**: the run window starts the server by itself.

---

## 17. Advanced Tools

`emit()` for particles, `repeat()`, and `flash()` all work exactly as in 1.1. `schedule()` got a real upgrade, and randomness got several.

### Cancellable Timers

```tiberscript
respawnTimer = schedule(180) = function
  respawnPlayer()
end

if key("escape").hit then
  cancelSchedule(respawnTimer)
end
```

`schedule()` now returns a handle. Hold onto it, and `cancelSchedule()` can call the whole thing off before it fires. Useful anywhere a timed event might need to be interrupted, like a respawn the player prevents by finishing the level first. The number in `schedule(...)` can be any expression, `schedule(floor(delay / 2))` included, as it can in 1.1.

### A Bigger Randomness Toolkit

`random(6)` and `random(1, 6)` still work exactly as in 1.1, returning a whole number. `random` stays a plain function rather than becoming a namespace like `math` or `array`, on purpose: it was already taken as a function name in 1.1, and turning it into something with dot methods on top of still being callable on its own would be confusing rather than clean. Everything new below is its own clearly named function instead.

```tiberscript
seedRandom(1234)
n = random(1, 6)

f = randomFloat(0, 1)
coin = randomBool()
pick = randomChoice(["sword", "shield", "potion"])
dir = randomSign()
```

`seedRandom(seed)` makes every following call to `random()` and the functions below produce the exact same sequence every time. Useful for testing, for reproducing a bug a player reports, and for any replay or daily challenge system where everyone needs to see the same random layout. `randomFloat(min, max)` returns a decimal instead of a whole number, which `random()` has never done. `randomBool()` is a coin flip. `randomChoice(list)` picks one item out of an array at random. `randomSign()` returns `1` or `-1`, useful for a random direction without writing the coin flip by hand every time.

Randomness used inside `fixedUpdate()` is part of the Timeline: rewind a second and the same dice come up again, which is exactly what makes a rewind honest.

### Noise

`random()` is the right tool for a dice roll. For a landscape, a cloud, a flickering torch or a wandering enemy, you want values that change smoothly, and that is noise:

```tiberscript
for x = -200 to 200 by 4 do
  local h = noise(x * 0.02) * 40
  screen.fillRect(x, -100 + h / 2, 4, h + 20, "#2E5C38")
end

torch.brightness = 0.8 + noise(time.now * 0.05) * 0.2
```

`noise(x)`, `noise(x, y)` and `noise(x, y, z)` return a number from -1 to 1 that drifts smoothly as its inputs change: nearby inputs give nearby values. It follows `seedRandom()`, so a seeded world is the same world every time, and it is part of the Timeline like every other random number.

### Pathfinding

```tiberscript
route = path.find(level, from = vector(guard.x, guard.y), to = vector(player.x, player.y))
if route then
  guard.moveToward(route[1], 1.5)
end
```

`path.find()` finds the shortest walkable route across a `tilemap`, around its solid tiles, and returns it as a list of points, or `null` when there is none. `route[0]` is where the search started and `route[1]` is the next step to take. `path.find(level, ..., diagonal = false)` keeps to four directions. It is the A* search every game with a chasing enemy ends up writing, and getting slightly wrong, built in once and done right.

---

## 18. Tooling: Autocomplete and Documentation

Not a language feature. An editor one. Every built in function in 1.2 now carries a known signature: what it takes, in order, and what it returns. That is what makes it possible for the editor to actually suggest something the moment you type `screen.` instead of showing nothing: the keywords, the built in names, the members of each namespace, and your own project's functions and archetypes, each with what it takes. The editor also colors every keyword of 1.2 (`elseif`, `swarm`, `retro`, `forecast` and the rest), every object of the engine (`time`, `physics`, `lighting`, `capture`), and every built in function where it is called, so a name the engine knows looks different from a name of your own as you type it. Every code block in this handbook is also a test: each one is loaded and run on the 1.2 engine, so the handbook and the engine cannot quietly drift apart the way `for...in` briefly did before this release.

---

## 19. THE BIGGEST UPDATE OF ALL: Time, Swarms, the Forge (JavaScript and WebAssembly), the Graphics Chip, HTML, and Changing the Past

Every game is a story told one tick at a time, by thousands of little things at once. Every language before this one treats both of those as somebody else's problem. The past is thrown away as it goes. A crowd of ten thousand bullets is ten thousand separate objects the language has to chase around memory.

TiberScript 1.2 makes both part of the language, then makes the language fast enough to carry them, then hands the picture to the second processor every computer has, then writes the pages around the game in the game's own file and keeps them up to date, and then opens the past itself, so it can be changed.

- **Part One: Time Is a Value.** Your game can rewind itself, look into its own future, try "what if" without it happening, replay any moment, and play online with rollback, in one word, as plain TiberScript.
- **Part Two: A Million Things at Once.** `swarm of bullet` stores a whole crowd as a few blocks of numbers instead of a heap of objects: a fraction of the memory, next to no garbage, and a snapshot the Timeline can take in a third of a millisecond.
- **Part Three: The Forge: JavaScript and WebAssembly.** TiberScript compiled to JavaScript, the language the browser runs fastest: 67 to 492 times faster than 1.1 on its benchmarks, 18 to 25 times faster on a real game's frame and 345 times faster on a heavy one, and your code changed while the game runs without losing where you are. JavaScript is native to it: a whole block of it between `javascript` and `end`, a function or a method written in JavaScript, every name of the page through `js`, promises a sequence waits for, and callbacks both ways. One word, `fast`, has the Forge write WebAssembly instead: a crowd moved with `each fast` runs about five times faster again, and its math gives every machine exactly the same answer. And buffers, `wasm` and `Tiber.forge` put the three languages to work on the same numbers, and the Forge's own work in the game's hands.
- **Part Four: The Graphics Chip.** Put `shader` in front of a function, and the Forge writes it in the graphics chip's own language too. It runs once for every pixel, thousands at a time: a sky, water, fire, clouds that nobody painted, and the whole frame bent through a ripple or a black hole, in about a millisecond for a 1080p screen. `swarm.drawPoints()` hands a whole swarm to the chip and draws a million members in 5 ms.
- **Part Five: HTML, Powered by the Whole Forge.** `html shop ... end` writes a page of the game in the game's own file, HTML as the web writes it. `$(gold)` in it is a TiberScript value the page always shows as it is now, `do="buy(item)"` runs the game's own code on a click, `bind="volume"` keeps a slider and a value equal both ways, and `show=` and `each=` show a part only while it is true and repeat one for every item of a list. The Forge compiles every piece of it with the rest of the game, works it out after every update, and sends the page only what changed: a shop, a settings screen, a bag of items or a level editor, written the way the web writes them and run as part of the game.
- **Part Six: Change the Past.** The biggest of them all. `retro seconds(2) do ... end` makes a change two seconds ago, then plays every kept tick since again, with the same inputs and the code as it is now: the present becomes the one that change would have made. Saving your code while the game runs, or moving a slider, does the same over all the time kept, so a fix reaches back into the run you are playing. What had happened is kept beside it, to undo or to see faintly over the new present.

They are one feature, not six. Time is what turns any moment of a game into something it can rewind, forecast and replay, and a bug a player found into something you can watch happen again. Swarms are what make keeping ten seconds of a bullet hell cheap enough to do. The Forge is what makes running a game's logic many times a frame possible at all, and `fast` is what makes a crowd of a million, and a replay that plays the same on a phone as on the laptop that recorded it, possible on top of that. And the Graphics Chip is the last piece: the Forge that compiles a game to JavaScript and WebAssembly now writes the third language a computer speaks, a shader keeps nothing so the Timeline rewinds the picture with the game, and a swarm of a million is not only kept and moved but seen. And HTML is the face of all of it: the pages around the game, written in the same file and compiled by the same Forge, reading whatever present the game is in, so a rewound game shows the page it had then. And Changing the Past is where the other five meet: Time's kept inputs, played again hundreds of ticks inside one frame by the Forge and `fast`, crowds and all, drawn by the chip from whatever present they make, and shown on every page of the game at the next update.

TiberScript is still one language. You never have to write JavaScript, WebAssembly or GLSL to get any of this: they are the three languages the Forge writes for you, one for everything, one for the code you mark `fast`, and one for the code you mark `shader`. And when you do want to write one of them yourself, each is a word away: `javascript` (19.19), `glsl` (19.36), and `wasm` for WebAssembly of your own (19.24). HTML is the one you write yourself, because a page is meant to be written: inside it, every value, click, condition and list is TiberScript, and the Forge compiles those with the rest of the game (Part Five).

Because a TiberScript game already separates what changes the world (`fixedUpdate()`) from what reads input and draws (`update()`), and because its input already runs through `action()` and its randomness through `seedRandom()`, none of this needs you to write your game in a special way. Write it the way this handbook already shows, and you get all six.

### Part One: Time Is a Value

The moment tick 500 is computed, every other language forgets tick 499, and tick 501 is a mystery until it happens. TiberScript keeps the story. That means your game can:

- **rewind** itself, a second, a minute or all the way back to the start, to the exact frame,
- **forecast** its own future without living it,
- ask **what if**, try something, and throw the attempt away,
- **replay** any moment, at any speed, from any angle,
- play **online** in one word, with rollback and not a line of netcode,
- let you **debug backwards**, and **change the past** itself (Part Six).

#### 19.1 Keep Time

```tiberscript
init() = function
  time.keep(seconds(10))
end
```

That is the whole setup. From now on the engine remembers the last ten seconds of your game: every archetype instance, every registered object, every `data` block, every global your `fixedUpdate()` changes, the state of the random numbers, and every action on every tick.

`time.now` is the current tick, counted from the start of the game. `time.kept` is how many ticks back you can go right now, and `time.oldest` is the oldest tick you can go back to.

Leave the length out, and the engine keeps the whole game, from the moment `time.keep()` is called:

```tiberscript
init() = function
  time.keep()
end
```

Keeping a whole game does not mean keeping a copy of every tick. The engine keeps three things. The last few seconds are kept tick by tick, so a short rewind is instant. Further back, it keeps a picture of the world every so often, between a sixth of a second and two seconds apart (it chooses the spacing from how long one of your game's ticks takes), and every action pressed on every tick in between, which is tiny. Going back to a tick between two pictures starts from the picture before it and plays the recorded actions forward to that exact tick. The world it arrives at is then checked against a fingerprint taken when that tick first happened, so a rewind always shows the world as it really was, never a guess.

A picture only stores what changed since the one before it, so a game where most things stand still costs very little. There is still a limit, the memory the Timeline may use, which is 256 MB unless you give another number:

```tiberscript
init() = function
  time.keep(memory = 64)
end
```

When the pictures reach that limit, the Timeline thins out the oldest ones first, keeping every other picture, so the far past takes a little longer to reach but is still there. Only when reaching the oldest tick would take too long to fit inside a frame does the oldest stretch go. `time.keep(seconds(10), memory = 32)` gives a length and a limit together. The debug overlay (19.8) shows how many seconds are kept at any moment.

How far is far? The example that ships with microStudio, `4-biggest-update.tbs`, keeps its whole game with `time.keep()`. After a minute of play, holding the rewind button went back through 58 of those 60 seconds, and no single tick of the rewind took longer than 7.7 ms, well inside a frame. A scene with a thousand sparks on screen kept ten minutes of play in about 80 MB and rewound all of it, back to its first tick, with no tick of the rewind over 6 ms.

So time has as much or as little room as you give it: a length (`time.keep(seconds(30))`), a memory limit (`memory = 64`), both, or neither, for the whole game. `time.memory` says how many megabytes it holds right now.

Time can also be kept only in some states. A title screen, a menu or a cutscene usually should not be rewound into:

```tiberscript
states = enum
  title
  play
end

init() = function
  transit(states.title)
  time.keep(during = states.play)
end
```

With `during`, time is kept only while the game is in that state (or one of a list of them, `during = [states.play, states.boss]`). Each time the game goes into one, time starts afresh there, so a rewind stops at the moment play began and never takes the player back into the title screen. Out of those states nothing is kept and the memory is let go, `time.paused` is true, and `time.rewind()` simply does nothing.

Everything the Timeline keeps comes back exactly as it was on that tick: the world, the random numbers, a tween halfway through, a timer counting down, and the camera. So when you let go of rewind, the game and the view carry on from that exact moment, with no slide from wherever the camera had drifted. Holding rewind at the oldest tick kept holds the world still there.

What changes between two ticks, outside `fixedUpdate()`, is kept as well: a value `update()` or `draw()` sets, a `ui.button` clicked, a sequence moving on. It is kept with the next tick's actions, so playing time forward again from a picture changes it the same way, and a rewind still reaches as far back as time was kept. If a game still cannot be played forward again exactly, for example because a `javascript` block changes the world, the first rewind that notices stops at the furthest tick it can still reach exactly, and says why once, in the console: `time.rewind() stopped at tick 812: played again, the world does not come out the same`. The short rewind of the last few seconds keeps working in every game.

#### 19.2 Rewind

```tiberscript
init() = function
  time.keep(seconds(10))
  bind("rewind", "r")
  bind("rewind", gamepad(0).button("lb"))
end

fixedUpdate() = function
  if action("rewind").held then
    time.rewind(2)
  else
    applyPhysics("player")
    moveEnemies()
  end
end
```

Hold R and the whole game runs backwards at double speed. Let go and it carries on from there, as if the last few seconds never happened. That is the entire mechanic of a time rewind puzzle game, in five lines, and it is exact: not an approximation that drifts, the world as it really was on that tick.

`time.rewind(n)` goes back `n` ticks. `time.rewindTo(tick)` goes straight to one tick (a tick older than `time.oldest` goes to `time.oldest`). Give a tick a name with `time.mark(name)`, and `time.rewindTo(name)` goes back to it, for a button that jumps back to the start of the level or to a checkpoint:

```tiberscript
entering states.play do
  time.mark("levelStart")
end

fixedUpdate() = function
  if action("restartLevel").hit then
    time.rewindTo("levelStart")
  end
end
```

Three more give you the rest of the say over time. `time.forget()` lets go of everything kept so far, so a rewind goes back no further than the moment it was called (after a checkpoint, say). `time.stop()` stops keeping time altogether. And `time.atOldest` is true when a rewind has gone as far back as time was kept, for a "no further" sound or message.

`time.rewinding` is true on the ticks a rewind happened, for a sound or a screen tint:

```tiberscript
update() = function
  if time.rewinding then
    alter opacity=0.25 do
      screen.fillRect(0, 0, 400, 250, "#5AA0FF")
    end
  end
end
```

Some things must not rewind. The settings menu's volume, the number of rewinds a player has used, the achievement they just earned. Mark those `timeless`:

```tiberscript
timeless rewindTicks = 0
timeless settings = data
  musicVolume = 0.6
end

hero = archetype
  timeless deaths = 0
end
```

A `timeless` value keeps whatever it was set to last, no matter where in time the game goes.

**At a part of a tick's speed.** The number can be less than one. `time.rewind(0.5)` goes back one tick every other frame and holds the world still in between, so the game runs backwards at half speed; `time.rewind(0.25)` at a quarter. Any number works, 1.5 as well as 0.1: what is left of a tick waits for the next call.

**Forward again.** After going back, `time.forward(n)` goes forward again over the same ticks, exactly as they were, as long as no new tick has been played since: `time.ahead` says how many ticks there are to go forward over. Together they are a scrubber: hold one key to run the game backwards, another to run it forwards again, at any speed, and let go to play on from wherever that is.

```tiberscript
init() = function
  time.keep(seconds(20))
  bind("back", "left")
  bind("forth", "right")
  bind("slowly", "shift")
  ball = register("ball", 0, 0)
end

fixedUpdate() = function
  local pace = 1
  if action("slowly").held then pace = 0.25 end
  if action("back").held then
    time.rewind(pace)
  elsetry action("forth").held and time.ahead > 0 then
    time.forward(pace)
  else
    ball.x += 2
  end
end
```

The moment a new tick is played, what was ahead is another future, and `time.ahead` is 0 again.

#### 19.3 Forecast

`time.forecast(n)` runs the game `n` ticks ahead, in private, and hands you the world as it will be. Nothing actually happens: no sound plays, nothing is drawn, the real game does not move.

```tiberscript
update() = function
  local ahead = time.forecast(30)
  if ahead.player.hp < player.hp then
    screen.text("DANGER", 0, 80, 16, "red")
  end
end
```

The world it gives back reads exactly like the real one: `ahead.player.x`, `ahead.boss.hp`, `ahead.count("enemy")`. It is a picture of the future, so it cannot be changed, only looked at.

To watch the future unfold tick by tick, for drawing a path, forecast with `each`:

```tiberscript
forecast 60 each (future) do
  screen.fillCircle(future.ball.x, future.ball.y, 1.5, "white")
end
```

That is the aiming line of every physics puzzle and golf game, the dotted arc of a grenade throw, the ghost of where the boss is about to dash, and it is not an estimate someone had to write by hand with their own copy of the physics. It is the real physics, the same code the game runs, run ahead.

Running sixty ticks ahead on every frame would cost sixty ticks on every frame, so a `forecast ... each` is kept from frame to frame. A tick later, the future it saw is still the future: when the world is exactly the tick the forecast said it would be, with the same actions held, the ticks already seen are kept and only the newest one is played. What the block read of each future tick is kept too (only the values it read, a few numbers), so drawing an older tick again does not need the world as it was. When the world did something else, a key pressed or let go, the forecast starts again and plays only as many ticks as fit in 5 ms of that frame, the rest over the next frames. So the path may be short for a frame or two, but the game never slows down. A block that calls a method of the future (`future.count(...)`) or loops over its names is played whole on every frame, as before.

Measured on a scene where a ship draws its path 40 ticks ahead while a thousand sparks fly: played whole every frame, the forecast took 12.5 of the 14 ms each frame cost, and the frame rate fell as the sparks grew. Kept from frame to frame, the whole frame takes 1.6 ms, and every frame draws exactly the path playing it all again would draw, which the tests check on 400 frames of changing input.

One thing is different in a tick that is only worked out, a forecast's, a `whatif`'s, or one a rewind or a change to the past plays again: `key(name).hit` is false there. A key goes down once, on the real tick it was pressed, so a setting switched straight from `key("m").hit` switches once, and not once more for every tick played ahead. `key(name).held` still says what is held, and actions, which the Timeline records, are replayed exactly as they were.

**The world a while ago.** A forecast is a picture of the future. `time.ago(n)` is the same kind of picture of the past: the world as it was `n` ticks ago, to read and draw, with nothing in the present changed. It is the ghost that follows the hero a second behind, the trail of where a boss has been, the replay shown small in a corner.

```tiberscript
init() = function
  time.keep(seconds(5))
  hero = register("hero", 0, 0)
end

fixedUpdate() = function
  hero.x += 1.5
end

draw() = function
  screen.clear("#101020")
  local was = time.ago(seconds(1))
  alter opacity=0.35 do
    screen.fillRect(was.hero.x, was.hero.y, 12, 12, "#5AA0FF")
  end
  screen.fillRect(hero.x, hero.y, 12, 12, "#FFFFFF")
end
```

Farther back than time is kept gives the oldest tick kept, and like a forecast the picture cannot be written on.

#### 19.4 What If

Forecasting shows the future if nothing changes. `whatif` shows it if something does, and then undoes it:

```tiberscript
update() = function
  if action("aim").held then
    whatif do
      ball.vx = aim.x * power
      ball.vy = aim.y * power
      forecast 90 each (future) do
        screen.fillCircle(future.ball.x, future.ball.y, 1.5, "white")
      end
    end
  end
  if action("aim").released then
    ball.vx = aim.x * power
    ball.vy = aim.y * power
  end
end
```

Inside `whatif`, the world is a copy. Change anything, forecast it, draw from it, and when the block ends the copy is gone and the real game is exactly as it was. A `whatif` can `return` a value, which is how a game makes decisions by trying them:

```tiberscript
// a boss that looks before it leaps: try each dodge, keep the safest
safestDodge() = function
  local best = 0
  local fewest = 999
  for dir in [-1, 0, 1] do
    local hits = whatif do
      boss.vx = dir * 4
      return time.forecast(30).boss.hitsTaken
    end
    if hits < fewest then
      fewest = hits
      best = dir
    end
  end
  return best
end
```

That is planning AI: an enemy that simulates its own options with the game's real rules and picks the best one. In other languages it means writing a second, simplified copy of your whole game for the AI to think with. In TiberScript it is ten lines, and the AI can never be fooled by a copy that disagrees with the real thing.

#### 19.5 Replay

```tiberscript
on("bossDefeated") do (info)
  time.replay(from = time.now - seconds(3), speed = 0.25)
end
```

The last three seconds, again, at quarter speed. That is a kill cam. During a replay the game draws exactly as it did the first time, `update()` included, while `fixedUpdate()` is driven by the recording instead of by the player. `time.replaying` is true while it plays, so the HUD can say REPLAY and the camera can do something dramatic.

Replays can also be saved and loaded, because a replay is small: the starting world plus the actions on each tick. `time.saveRecording("run")` also keeps it under its name, for `test ... replay "run"` (19.6).

```tiberscript
time.record("run")
// ... play ...
time.stopRecording()
save.set("bestRun", time.recording("run"))

// later, or on someone else's machine
time.replay(save.get("bestRun", null))
```

Ghost races, attract mode demos on the title screen, "watch how the developer beat this level", and the most useful thing of all, a player sending you the exact run where the bug happened.

#### 19.6 Tests That Play

Section 12's tests check functions. With the Timeline, a test can play your game:

```tiberscript
test "the boss never gets stuck in a wall" replay "bug report 17" do
  every(1) do
    assert(boss.x > -200 and boss.x < 200)
  end
end

test "a perfect run still wins" replay "developer best" do
  assert(time.ended.state == states.victory)
end
```

`test ... replay "name"` runs a saved recording from start to finish, as fast as the machine can, checking as it goes. A bug a player found once becomes a test that proves it never comes back. It plays in a copy of the game made for it, from the start, with the recording's actions: the body is checked after every tick, and `time.ended` is the world at the end. `Tiber.runTests()` runs these with the rest.

#### 19.7 Rollback Multiplayer, and Online in One Word

Action games online have one enemy: the time it takes a message to cross the internet. The best answer the games industry has, rollback netcode, used to be something only fighting game studios could build. Each player's game runs ahead on its own guesses; when the other player's real input arrives late, the game rewinds to the tick it belongs to, applies it, and fast forwards back to now, before the next frame is drawn.

In TiberScript that machinery already exists: it is `time.rewind`, and forecasting forward again. So rollback is one line:

```tiberscript
init() = function
  network.rollback(players = 2, delay = 2)
end
```

That is all. Every action of every player is sent for you. Late ones rewind and replay the world for you. There is no `network.send` of positions, no interpolation, no "who is right" code, because every machine runs the same game with the same inputs and ends up in the same place. The only rule is the Timeline's rule: the world changes in `fixedUpdate()`, input comes through actions.

Each player's input is read with `action(name, player)`, players counted from 0, so a two player game moves `heroes[0]` with `action("right", 0)` and `heroes[1]` with `action("right", 1)`; `action(name)` alone is this machine's own player, `network.me`. The match starts once every player is connected, with the same random numbers on every machine, and every machine plays the same ticks: two players, one of them seven ticks away, were played side by side and every tick came out the same on both.

Rollback is how a game plays online. `online` is everything else playing online needs, in one word:

```tiberscript
init() = function
  bind("right", "arrowRight")
  bind("left", "arrowLeft")
  online
  heroes = [register("blue", -40, 0), register("orange", 40, 0)]
end

fixedUpdate() = function
  for p = 0 to 1 do
    if action("right", p).held then heroes[p].x += 2 end
    if action("left", p).held then heroes[p].x -= 2 end
  end
end
```

`online` puts the game on the project's server and finds it a room. Until the room is full, the game waits in a lobby drawn over it: ONLINE, who is in ("waiting for players: 1 of 2"), the room's code, its link, and a QR code of the link, so a second player opens the link in another window or points a phone at the screen. Once every seat is taken the match starts with rollback, two ticks of delay and the same random numbers on every machine, and `action(name, player)` reads each player as above. `online 4` is a game for four, up to sixteen. In the editor you press Run and nothing else: the run window turns the project's networking option on and starts its server by itself.

Rooms fill their seats in the order players come. A link with `?room=CODE` opens that room; without one, a player goes to the first room of the same size that is still waiting, then to a game in progress with a free seat, and otherwise starts a room of their own, so one server holds as many games as there are pairs of players, each played apart. A player who comes to a full room watches: the same game, handed the world as it is, with WATCHING at the top of the screen. A player who leaves gives no input from their last tick on, on every machine alike, and the others are told ("player 2 left") and play on in step; someone who comes later takes the free seat and is handed the world as it is, at the last tick every input is known for. A connection that drops is opened again by itself, and joins the same room.

What the game can ask: `online.started` (the room is full and the game is on), `online.waiting` (in the lobby), `online.watching`, `online.me` (this machine's seat, from 0, or -1 when watching), `online.players`, `online.seats`, `online.watchers`, `online.room` (its code), `online.link` and `online.status` (`"connecting"`, `"waiting"`, `"joining"`, `"playing"` or `"watching"`). The example `5-online.tbs` that comes with microStudio is a whole game: two ships, the first to catch five stars wins.

It was checked two ways. The tests play rooms of quick and slow players through a relay held in memory: the lobby, two rooms at once, a third player watching, a player leaving and another taking the seat, and every machine of a room ending every tick on the same numbers. And in microStudio itself, the example's game in the run window and a second window raced to five stars through the project's own server, one window left and a new one took its seat and was handed the race, and both windows ended on exactly the same numbers.

#### 19.8 Debugging Backwards, and Editing the Past

With `Tiber.showDebugOverlay(true)`, the overlay gains a timeline bar. Drag it back and the game shows that tick: click any object to see every field it had then. Step back one tick at a time and watch the exact frame a value went wrong, instead of guessing and adding `debug()` calls.

Then fix the code, and save. The Forge recompiles only what changed, and the Timeline plays every kept tick up to where you were looking again with the new code: the game is where it was, as if it had always been written that way, and runs forward from there. You see your fix apply to the moment the bug happened, and to everything before it, without restarting and playing back to it by hand. That is Part Five, changing the past, and 19.42 says all of it.

#### 19.9 The Rules of Time

Time travel is exact because a TiberScript game is **deterministic**: the same starting world and the same inputs always give the same result. 1.2 keeps it that way with four rules, and the engine checks all four for you.

1. **The world changes in `fixedUpdate()`.** That is where a game's rules belong, and a network game (19.7) needs them there, since every machine plays only `fixedUpdate()` the same way. In a game played on one machine, what `update()`, `draw()`, a button or a sequence changes anyway is kept with the next tick (19.1), so time still rewinds it exactly.
2. **Input comes through the engine.** `action()`, `key()`, `gamepad()`, `mouse`, and `touch` are all recorded per tick. Anything else from outside, the clock on the wall or a message from the network, arrives as an input too: `time.wallClock` rather than a date.
3. **Randomness is seeded.** Every random number in `fixedUpdate()` comes from the Timeline's own random stream, which rewinds with everything else. `seedRandom()` still works, and starts that stream where you say.
4. **Things outside the world are `timeless`.** Settings, menus, statistics, anything that must not move when time does.

A game that follows the rules gets every part of this section. A game that does not, a 1.1 game that moves everything in `update()`, runs exactly as it always did, and the Timeline simply stays off until it is asked for.

Changing the past (Part Six) plays kept ticks again, so it asks nothing more than these four rules: a game that follows them can change its past exactly.

What time does not move: a running `sequence` or `dialogue` carries on from where it is, tiles changed with `level.set()` stay changed, and what a `javascript` block did outside the game stays done. Sounds and particles are what the player sees and hears, not the world, so they are not kept either. A forecast and a `whatif` put back everything, `timeless` values included.

**How fast time goes.** `time.speed` is how many ticks the game plays a frame: 1 as it starts, 0.5 for half speed, 0.25 for a quarter, 2 for twice as fast, anything from a sixteenth to 8. Whole ticks are played, fewer or more of them, so a slowed game is the same game: it keeps time, rewinds and replays exactly as at full speed. In between two ticks `draw()` shows the world part of the way from one to the next, so slow motion is smooth and not a slide show (`time.smooth = false` shows one picture a tick). A key or a button pressed on a frame that plays no tick is not lost: the next tick sees it.

```tiberscript
init() = function
  time.keep(seconds(10))
  bind("focus", "shift")
  hero = register("hero", 0, 0)
  hits = 0
end

fixedUpdate() = function
  hero.x += 3
  if hero.x > 150 then
    hero.x = -150
    hits += 1
    time.freeze(seconds(0.1))
  end
end

update() = function
  if action("focus").hit then time.slow(0.25, seconds(2), ease = seconds(0.5)) end
end
```

| What | What it does |
| --- | --- |
| `time.speed` | how fast time goes, from 0.0625 to 8; setting it holds until it is set again |
| `time.slow(speed, ticks, ease = ticks)` | that speed for that long, then back to the speed it had by itself; with `ease`, it glides back over the last ticks |
| `time.freeze(ticks)` | the world stops for that long and starts again by itself: the pause of a heavy hit |
| `time.step(n)` | while time is slowed or stopped, plays `n` ticks now, one a frame |
| `time.resume()` | the game's own speed again, at once |
| `time.slowed`, `time.frozen` | true while time goes slower than 1, and while it is stopped |
| `time.smooth` | true to start: the world is drawn part of the way between two ticks while time is slow |

The lengths given to `time.slow()` and `time.freeze()` are counted on the clock on the wall, not in the game's slowed ticks, so two seconds of slow motion last two seconds. A stop always ends by itself, which is why `time.speed` cannot be set to 0: nothing of the game runs while the world is stopped, so nothing could start it again. What must go on while time is slow, a pause menu or a gauge that drains, is drawn and read in `draw()`, which runs every frame whatever the speed. In an online game (19.7) every player's machine plays the same ticks, so time has one speed there and these do nothing.


### Part Two: A Million Things at Once

Bullets, sparks, coins, snowflakes, a flock of birds, a crowd in a stadium. Games are full of things there are thousands of, and every high level language stores each one as its own object somewhere in memory. Past a few thousand, three things go wrong at once: the objects take far more memory than their numbers need, the garbage collector stops the game to clean up the ones that died, and the Timeline has to copy each one separately to remember it.

The big engines' answer is a different way of writing games altogether, an entity component system, with its own vocabulary and a rewrite of everything you already had. TiberScript's answer is one word.

#### 19.10 swarm

```tiberscript
bullet = archetype
  init(x, y, vx, vy) = function
    this.x = x
    this.y = y
    this.vx = vx
    this.vy = vy
    this.life = seconds(2)
    this.mark("bullet")
  end
end

bullets = swarm of bullet
```

`bullet` is an ordinary archetype. `bullets` is a swarm of them: a crowd that stores every member's `x` in one block of numbers, every `y` in another, and so on, instead of scattering thousands of separate objects through memory.

```tiberscript
fire(x, y, angle) = function
  bullets.spawn(x, y, cos(angle) * 3, sin(angle) * 3)
end
```

`bullets.spawn(...)` makes a new member and runs its `init()` with those arguments, exactly like `new bullet(...)` would. It returns the member, so you can set more on it. `bullets.count` is how many are alive right now, and `bullets.clear()` despawns them all, for a new round. A swarm grows as it needs to; `swarm of bullet max 20000` sets a ceiling, and a spawn past it is ignored with a warning, which is what a bullet hell wants rather than a crash.

Nothing about the archetype changes. Its methods, its traits, its tags and its `extends` all work on swarm members, and the same archetype can still make ordinary single objects with `new` somewhere else in the same game.

#### 19.11 each

```tiberscript
fixedUpdate() = function
  bullets.each do (b)
    b.x += b.vx
    b.y += b.vy
    b.life -= 1
    if b.life <= 0 then b.despawn() end
  end
end
```

`each` runs its block once for every member. Inside it, `b` reads and writes like any object, and the Forge turns the whole block into one tight loop over those blocks of numbers, with no object made for `b` at all.

`b.despawn()` removes a member. Doing it in the middle of `each` is safe: the member finishes the block and is gone after it. A member spawned during `each` joins after the pass, so a loop never chases its own tail. Holding onto a member after it has been despawned is an error that names the line, not a silent read of whatever reused its place: `b.alive` says whether it is still there.

#### 19.12 where

Most of the time you want some of a crowd, not all of it. `where` works on any `for ... in` loop, over a swarm or over an ordinary list:

```tiberscript
for b in bullets where b.y > 100 do
  b.despawn()
end

for e in enemies where e.hp < 10 and distance(e.x, e.y, player.x, player.y) < 60 do
  e.flee()
end
```

It reads the way you would say it, and on a swarm the Forge checks the condition straight against the numbers, without visiting a single object that does not match.

#### 19.13 touching

The single most common thing a crowd does in a game is hit something. `each ... touching` gives you every overlapping pair, found with the spatial grid, never by checking every bullet against every enemy:

```tiberscript
fixedUpdate() = function
  bullets.each touching "enemy" do (b, e)
    e.takeDamage(1)
    emit("hit", b.x, b.y, 3)
    b.despawn()
  end
end
```

`collide("bullet")` and `near("bullet", 40)` also see swarm members, exactly like any other tagged object, so code that already worked with a handful of bullets keeps working with fifty thousand.

`collide()` and `near()` are quick with a crowd of ordinary objects too. The engine keeps a list of the objects that carry each tag, updated whenever a tag is given or taken away, so `collide("coin")` looks only at the coins, never at every object in the game.

#### 19.14 Drawing a Swarm

```tiberscript
update() = function
  bullets.draw("bullet_sprite")
  sparks.draw(circle = 1.5, color = "#FFD750")
end
```

One call draws every member, with a sprite or a shape. Circles go to the screen as one shape for the whole crowd, a single fill instead of one per member. Two crowds of 10,000 dots drawn one dot at a time bring a game down to 6 or 7 frames a second; drawn as one shape each, the whole frame takes under 4 ms in a browser. A member that needs to look different from its neighbours can have a `draw()` method of its own, and a swarm whose archetype has one uses it instead. `emit()`'s particles have always been a swarm the engine kept to itself. Now yours are the same thing.

#### 19.15 Swarms and Time

This is why swarms are Part Two of the biggest update and not a line in the standard library. A swarm is a few blocks of numbers, and the Timeline keeps a world by copying it. Copying a block of numbers is the single fastest thing a computer does. A game with a hundred thousand bullets on screen can keep ten seconds of history, rewind through a bullet storm, forecast where every bullet will be half a second from now, and roll back six ticks for a late network packet, and every one of those is a handful of block copies.

The rules of a swarm follow from what it is:

1. **A member's fields are the ones `init()` gives it.** The shape is fixed, the same rule the Forge already uses to make archetypes fast. Setting a field `init()` never mentioned is an error that names the field and says where to add it.
2. **Numbers are stored as numbers.** Numbers, booleans and enums go straight into the blocks. A text field is allowed, and kept to one side, so a swarm of numbers stays a swarm of numbers.
3. **A member is not kept after it is despawned.** Hold its values, not the member, if you need them later.

A game that breaks none of these, which is any game that treats its bullets the way this section does, gets everything above for nothing.

#### 19.16 Measured

A swarm was prototyped, like the Forge, to measure it rather than promise it. The workload is a bullet hell: bullets fly, age, leave the arena or hit one of 256 enemies found with a spatial grid, and every one removed is replaced, so the count never changes. The same program was written three ways, run to the same checksum in all three, and timed:

| Version | Bullets | Time per frame | Same answer |
| --- | --- | --- | --- |
| TiberScript 1.1 interpreter, a list of objects | 2,000 | 169.7 ms | yes |
| JavaScript, one object per bullet | 10,000 | 0.294 ms | yes |
| **swarm**, what the Forge generates | 10,000 | **0.282 ms** | yes |
| JavaScript, one object per bullet | 100,000 | 3.24 ms | yes |
| **swarm** | 100,000 | **2.99 ms** | yes |
| JavaScript, one object per bullet | 1,000,000 | 46.2 ms | yes |
| **swarm** | 1,000,000 | **36.0 ms** | yes |

Plainly: per bullet, a swarm is about 3,000 times faster than 1.1. Against JavaScript written well, it is only a little ahead on raw speed, 4 to 28 percent, because the browser is already very good at objects that all have one shape. A hundred thousand bullets fit in a 60 fps frame with most of the frame left over. A million do not: the logic alone runs at about 28 frames a second.

Raw speed was never the point. This is what changes, on the same 100,000 bullets:

| 100,000 bullets | Objects | Swarm | Difference |
| --- | --- | --- | --- |
| Memory | 12.5 MB | 3.4 MB | 3.7x less |
| Timeline snapshot | 2.80 ms | 0.37 ms | 7.6x faster |
| Timeline restore (rewind) | 0.61 ms | 0.09 ms | 6.8x faster |
| Median frame, 4,000 spawned and removed every frame | 2.26 ms | 1.34 ms | 1.7x faster |
| 99th percentile frame, same | 9.45 ms | 1.60 ms | 5.9x steadier |

The last line is the one players feel. With objects, one frame in a hundred takes four times longer than the rest while the garbage collector cleans up the bullets that died, and that is a stutter you can see. A swarm makes no garbage to collect. (The single worst frame in 600 was about the same for both, 11 to 13 ms, which is the machine, not the swarm.)

That was the swarm as prototyped. The swarm that ships keeps the same columns; its `each` reaches a member's fields through a small handle, and moving 100,000 members once (four fields each) takes about 4 ms with `each` and about 0.9 ms with `each fast`.

### Part Three: The Forge

#### 19.17 One Compiler Under Everything

Keeping ten seconds of a world, forecasting a second ahead every frame, rolling back six ticks when a packet arrives late, moving a hundred thousand bullets, playing a recorded run back as a test: all of it means running your game's logic many times over, and it only works if that logic is fast. 1.1 could not do it. It reads TiberScript a line at a time, as text, every time the line runs.

1.2 has the **Forge**. When you press Run, the Forge compiles every file to JavaScript, once, and the browser's own optimizing compiler turns that into machine code. Everything TiberScript knows about your game goes into making that code fast:

- **Only what runs is compiled.** Each file is read in one quick pass; each of its functions is compiled the first time it is called, and what was compiled is kept between Runs (in the browser's storage), so a Run where nothing changed compiles nothing and a Run after an edit compiles only the functions that changed.
- **The game starts first, the checking comes after.** On the first Run, the Forge compiles only what the first frame needs and starts the game. Reading every other function for mistakes happens a moment later, while the browser has nothing else to do, and a mistake anywhere is still reported, with its line, just as before.
- **What a game needs most is kept together.** The functions a game calls every frame, and its top level code, are kept as one piece of JavaScript. The browser turns that piece into machine code once and keeps the machine code for as long as the tab is open, so every Run after the first one in a tab starts from machine code that is already made.
- **Locals are locals.** A `local` and a loop's counter are JavaScript variables, and a file's own names live in one small object the whole file shares.
- **A number stays a number.** When a `local` only ever holds numbers, the Forge knows it, and its arithmetic, its comparisons and its `%` compile to plain JavaScript arithmetic, with none of the checks a value of unknown kind needs. When such a local is only ever used in arithmetic, even the step that turns a missing value or a number that is not a number into 0 is made once, where the local is given its value, instead of at every use. The same goes for a list's `push()`, `pop()`, `contains()` and `indexOf()`: they compile to the list operation itself, right where they are called.
- **Line numbers cost nothing.** Every error still names its line. The Forge only records which line is running where something could actually go wrong, not on every line of plain arithmetic.
- **A method is found where it is called.** `enemy.step()` looks up the archetype's method right at the call and calls it, with no trip through the engine. A call into a namespace, `math.floor()` or `vector()`, goes straight to the function the same way.
- **Math is the browser's.** `sqrt()`, `floor()`, `sin()` and the rest compile to the browser's own math functions.
- **Sequences are generators.** A `sequence` that waits compiles to a paused function, not a pile of timers.
- **Swarms are columns.** A `swarm` keeps one typed block of numbers per field; `each fast` runs its loop over them in WebAssembly (Part Two, 19.21).
- **Replayed tests run headless.** A test that replays a run plays it in a copy of the game with its drawing switched off.
- **A loop is left alone.** The watch for a loop that runs too long (Section 12) looks up once every few thousand turns, not on every turn, so inside the loop the browser sees nothing but the loop's own arithmetic.
- **A whole number is written once.** A score, a count, a timer in a HUD: the text of a whole number comes from a table made once, instead of being made again every frame it is drawn.
- **A method of a parent is found at once.** A call to a method an archetype has from its parent, or from its parent's parent, takes the same quick way as a call to one of its own.
- **What a game does not use costs nothing.** A frame of a game with no tweens, particles, lights, effects or filters never asks about them, and a game that never calls `Tiber.addCommand()` has its calls go straight to their functions, with no look for a custom command first. The look comes back the moment a game adds one, a line typed in the console included.

Next on that list: enums as small numbers and tags as bits, a spatial grid under `collide()` and `near()` (`each ... touching` already uses one), and vector math with no objects made.

**The benchmarks.**

Every number below was measured on one recent laptop, warmed up and taken as the middle of seven runs, with both engines exactly as they ship, run the way a browser tab runs them. Each program gave the same answer on both engines. On another computer every time changes, and how many times faster 1.2 is stays close.

The same six programs, each written once in TiberScript:

| Benchmark | What it measures | 1.1 interpreter | 1.2 (the Forge) | 1.2 vs 1.1 |
| --- | --- | --- | --- | --- |
| fib(25) | function calls, recursion | 290.3 ms | 1.19 ms | 245x faster |
| loops (1,000,000) | arithmetic in a tight loop | 620.0 ms | 9.31 ms | 67x faster |
| sieve (99,999) | lists, indexing | 108.3 ms | 1.25 ms | 86x faster |
| nbody (60 bodies, 30 steps) | floating point physics | 360.7 ms | 0.73 ms | 492x faster |
| entities (2,000 objects, 100 frames) | archetype instances and methods | 719.2 ms | 2.63 ms | 274x faster |
| strings (100,000) | building strings | 33.8 ms | 0.47 ms | 73x faster |

Five heavy frames, the kind a busy game has, each played for 300 frames on both engines:

| A frame of | 1.1, median | 1.2, median | 1.1, worst | 1.2, worst | Garbage collections, 1.1 and 1.2 | 1.2 vs 1.1 |
| --- | --- | --- | --- | --- | --- | --- |
| 2,000 enemies chasing the player, with collisions | 5.74 ms | 0.058 ms | 6.07 ms | 0.29 ms | 1,105 and 3 | 98x faster |
| 5,000 particles in a list | 5.05 ms | 0.035 ms | 9.56 ms | 0.10 ms | 440 and 0 | 144x faster |
| a HUD of 400 texts | 0.481 ms | 0.029 ms | 0.85 ms | 0.23 ms | 17 and 1 | 17x faster |
| a flood fill on a 24 by 24 grid, 4 times | 41.0 ms | 0.119 ms | 45.7 ms | 0.19 ms | 477 and 0 | 345x faster |
| 300 state machines calling methods | 0.769 ms | 0.007 ms | 1.08 ms | 0.04 ms | 7 and 0 | 110x faster |

The HUD is the smallest gain because most of its work is handing texts to the screen, which costs the same on both engines. Where a frame is your own logic, 1.2 is tens to hundreds of times faster, and it makes next to no garbage, so the garbage collector almost never has to stop the game.

And a real game: a platform game of 6,647 lines, with five bosses, crowds of enemies and menus, played with the same keys on both, each engine alone, as a tab has one game:

| A real game | 1.1 | 1.2 | 1.2 vs 1.1 |
| --- | --- | --- | --- |
| a frame's update and draw logic, median, in its first minute | 0.579 ms | 0.031 ms | 18x faster |
| the same, in its third minute | 0.569 ms | 0.023 ms | 25x faster |
| a frame, 99th percentile | 1.01 ms | 0.21 ms | 4.7x faster |
| the worst frame of 3,600 | 9.8 ms | 5.7 ms | 1.7x faster |
| garbage collections in a minute | 421 | 14 | 30x fewer |
| time spent collecting garbage in that minute | 35.0 ms | 3.0 ms | 12x less |
| Run again in the same tab, nothing changed | 25.4 ms | 6.1 ms | 4.1x faster |
| Run again in the same tab, after an edit | 25.6 ms | 9.5 ms | 2.7x faster |
| Run in a newly opened tab, nothing changed | 36.2 ms | 24.1 ms | 1.5x faster |
| Run in a newly opened tab, after an edit | 36.2 ms | 26.9 ms | 1.3x faster |
| a Run with nothing compiled before at all | 36.2 ms | 50.2 ms | 1.4x slower |
| saving a file while it runs | 11.8 ms | 2.7 ms | 4.3x faster |
| memory the game holds after a minute, the usual Run | 1.7 MB | 5.0 MB | 2.9x more |
| memory the game holds after a minute, nothing compiled before | 1.7 MB | 7.3 MB | 4.2x more |
| the engine the game's export carries, gzipped | 16 KB | 91 KB | 5.7x more |
| the engine with every part, gzipped | 16 KB | 142 KB | 8.9x more |

A frame of that game's logic costs 23 to 31 thousandths of a millisecond, about one fifth of one percent of a 60 fps frame. That is the room a ten year old laptop needs: even at a quarter of this speed, the whole of a frame's logic takes about a sixth of a millisecond.

The first minute and the third differ because of the browser, not the game. A browser turns a function into faster machine code in steps, after it has run a few hundred times and again after a few thousand, and a function that runs once a frame needs a minute or two to get there. Nothing stutters while it happens: a frame only gets cheaper.

"Run again in the same tab" is the Run you press over and over while you work, and that is where 1.2 gains the most at the start, because the browser still has the game's machine code from the Run before.

The first Run needs no waiting either. While a TiberScript project is open in the editor, the editor compiles each of its files in the moments you are not typing, a function at a time, and keeps the result where the Run looks for it. So the first Run of a game you have been working on starts from code that is already compiled, like any other Run. Only a Run with nothing compiled before at all (a project opened and run in the same instant) compiles what the first frame needs, and that one takes about a sixtieth of a second longer than 1.1, once.

The memory is the price of the speed, and it is honest to say so. Most of it is machine code: the browser turns the game's busiest functions into native code, and that code is what makes a frame 18 to 25 times faster. What does not buy speed has been cut: the Forge keeps each function's code in one piece instead of the many pieces it was built from, and writes calls on screen, audio and the rest of the engine's own objects in a fraction of the space. A Run that finds its code already compiled holds 5.0 MB, because it never needs the compiler at all.

The engine an exported game carries is 1.2's whole library: the compiler, the Timeline, swarms, controllers, networking and `fast`. An export carries only the parts the game's code can reach (a game with no `time.`, swarm, replay, `save.game()` or network in it leaves the Timeline and networking out, a game with no `fast`, no buffer and no `wasm` leaves out WebAssembly, and a game with no `shader` leaves out the graphics chip's part), gives the engine's own variables short names, and leaves out comments and indentation. For the game above that is 91 KB instead of 142 KB. Both columns are measured the same way (1.1's engine also shortened and stripped, down to 16 KB): 1.1 is a small interpreter, and 1.2 carries a compiler, which is what makes it tens to hundreds of times faster.

**Its three weaknesses, plainly.** Against 1.1, the Forge loses in three places. Each one comes from what makes it fast, so each is kept as small as it can be rather than taken away:

- **The very first Run is 1.4 times slower** (50.2 ms against 36.2 ms), because the Forge compiles a function the first time it is called. It happens once: every later Run uses what was compiled and kept, and in the editor a project's files are compiled while you are not typing, so a game you are working on never meets it. An exported game meets it the first time it starts on a computer, for a few hundredths of a second, and not again.
- **It holds about three times the memory** after a minute (5.0 MB against 1.7 MB). Most of it is the machine code the browser makes of the busiest functions, which is the speed itself. Five megabytes is a small part of what a browser tab uses, on an old laptop too.
- **Its exports are bigger**: 91 KB of engine, gzipped, against 16 KB, because the engine carries the compiler that makes the game fast. An export carries only the parts its code can reach, and the whole engine is smaller than one photo from a phone.

The Forge was prototyped before it was built, to measure it rather than promise it. The prototype compiled only the core of the language, and ran six programs within a few tens of percent of the same programs written by hand in JavaScript, which is the ceiling for anything in a browser. It kept none of 1.1's rules for the unexpected: a missing value reads as null, a list read past its end gives null, a text joined with a number is written the way TiberScript writes numbers, and every line is known when something goes wrong. The Forge that ships keeps every one of those rules, so every 1.1 program plays exactly as it did, and it still reaches that ceiling: wherever it can prove that a value is always a number, it compiles it with none of those checks (the list above). On 1.1 a frame of the game above is an interpreter walking every line, 0.63 ms of it. Compiled, the same logic costs 0.03 ms, which is the room the Timeline needs to forecast, rewind, and replay inside a single frame.

The swarm measurements are in Part Two. A fuller set of benchmarks is the next step, with more game shaped tests: pathfinding on a 256 by 256 grid, a hundred thousand particles, and the Timeline's own costs, a ten second rewind and a one second forecast of a full game.

#### 19.18 Change It While It Runs

Press Run once. After that, every time you save, the Forge compiles only the file that changed, in a few milliseconds, and swaps the new code into the running game. The game does not start over. The player stays where they are, the boss keeps its health, the wave keeps its count, and the next frame runs your new code.

```tiberscript
slime = archetype
  init(x) = function
    this.x = x
    this.speed = 1.5
    this.mark("enemy")
  end

  step() = function
    this.x += this.speed
  end
end
```

Change `this.x += this.speed` to `this.x += this.speed * 2` and save: every slime already on the screen speeds up, in the middle of the fight. The Forge knows every archetype's shape, so it knows what changed with it. A field the new `init()` adds is given its starting value on every living instance (the value it is written with there, when that is a plain number, text, `true`, `false` or `null`; otherwise `null` until your code sets it), and a field it no longer has is dropped.

One change cannot be made to a running game: renaming an archetype. The objects already made from the old name keep the old code, so the Forge says so as soon as you save, in the console: `The archetype 'slime' is no longer in main (renamed to 'blob'?), but 12 objects made from it are still in the game, with the old code. Press restart (the circular arrow) to start over with the new code`. Everything else in the file is already running the new code.

With the Timeline (Part One) this goes one step further: pause, drag back to the moment before the bug, change the code, and play forward from there with the new code (19.8). In 1.1, every change meant playing back to where you were, by hand.

#### 19.19 JavaScript, Native

The Forge writes TiberScript as JavaScript, so JavaScript is not a foreign language to TiberScript: it is the language TiberScript already becomes when it runs. TiberScript lets you write it yourself, wherever you need something the browser has and TiberScript does not wrap: a browser API, a library the page loaded, a date, a share button, a file on the web. Values go across as themselves, with nothing copied and no glue to write.

**JavaScript between two words.** `javascript` on a line of its own opens a block, and `end` closes it. Everything between the two is JavaScript in full: `let`, `const`, functions with values of their own, classes, arrow functions, template text, destructuring, regular expressions, anything the browser's JavaScript has. The editor colors it as JavaScript, exactly as it would in a JavaScript project, and goes back to TiberScript's colors at the `end`.

```tiberscript
score = 10
names = ["ada", "bob"]

javascript
  let combo = 0
  const TAU = Math.PI * 2

  function bump(n = 1) {
    combo += n
    score += combo
    return combo
  }

  class Bag {
    constructor(size) { this.size = size; this.items = [] }
    add(x) { if (this.items.length < this.size) this.items.push(x); return this.items.length }
    get full() { return this.items.length >= this.size }
  }

  const shout = (text) => text.toUpperCase() + "!"
  print(`${names.length} names, score ${score}`)
end

init() = function
  first = bump()
  second = bump(5)
  combo += 100
  bag = new Bag(2)
  bag.add("key")
  bag.add("map")
  loud = shout("hi")
end
```

The two languages share every name, both ways, with nothing to declare:

- **What the block declares is the game's.** Its `let`, `const`, functions and classes at the top of the block are names of the game like any other. TiberScript calls `bump()`, makes a `new Bag(2)`, reads `bag.full` and calls `shout("hi")` as if they were written in TiberScript. `first` is `1`, `second` is `6`, and `loud` is `"HI!"`.
- **What the game has is the block's.** `score` and `names` are TiberScript's, and the block reads and changes them by name. The built in functions are there too, `print()`, `floor()`, `clamp()`, `random()`, and so are `screen`, `audio`, `time` and the rest of the engine: a JavaScript function can draw with `screen.fillRect()`.
- **A variable is one variable.** `combo += 100` in TiberScript changes the very `combo` that `bump()` adds to. Nothing is copied from one language to the other, so nothing can fall out of step.
- **And the page is still there.** `Math`, `Date`, `navigator`, `fetch`, `localStorage`, any library the page has loaded: a name that is neither the block's nor the game's is the browser's.

A block at the top of a file runs once, where it stands, as the file loads: the `print()` above writes `2 names, score 10` before `init()` runs. A name nobody has, in the block, the game or the page, reads as `undefined` instead of stopping the game, so `typeof something === "undefined"` is a safe question to ask.

A block can stand inside a function too, and there it sees the function's locals and `this`, and can hand one value back:

```tiberscript
healer = archetype
  init(hp) = function
    this.hp = hp
  end
  drink() = function
    javascript
      this.hp = Math.min(100, this.hp + 30)
    end
    return this.hp
  end
end

sorted(list) = function
  local count = 0
  local result = javascript
    count = list.length
    return list.slice().sort((a, b) => a - b)
  end
  return result
end
```

`this.hp` in the block is the healer's own, the block changes the local `count`, and `local result = javascript ... end` takes what the block returns. A block in a function that uses `await` gives a promise, for `wait()` in a sequence or for `js.then()` (below). At the top of a file a block is not inside any function, so `return` and `await` at its own top level stop the file as it loads, with a message that says so.

A TiberScript object's methods are TiberScript's, so JavaScript reaches them through three small helpers: `Tiber.call(hero, "hurt", 5)` calls a method by its name, `Tiber.wrap(hero)` gives an object whose methods are called with a dot (`Tiber.wrap(hero).hurt(5)`), and `Tiber.make(hero, 50)` makes an object from an archetype, as `new hero(50)` does in TiberScript. `Tiber.globals` is every global of the game, for the rare block that has a name of its own in the way. Fields need none of this: `hero.hp` is `hero.hp` in both.

In a module (Section 3), what a plain block declares stays in its file, like everything else there, and `export javascript ... end` shares it with the files that import the module: `tools.hexColor()`.

What a block keeps in a `let` is a global of the game: a number, a text, a list or a plain object there is kept by the Timeline and rewinds with everything else. An object made from a JavaScript class is JavaScript's own, and time leaves it as it is, like the page's objects.

**A function or a method in JavaScript.** Put `javascript` in front of a function, and its body is JavaScript:

```tiberscript
javascript sortHigh(list) = function
  return list.slice().sort((a, b) => b - a)
end

knight = archetype
  init(hp) = function
    this.hp = hp
  end
  javascript heal(n) = function
    this.hp = Math.min(100, this.hp + n)
    return this.hp
  end
end

init() = function
  best = sortHigh([12, 99, 3, 47])
  k = new knight(90)
  k.heal(30)
end
```

It is called like any other function, from TiberScript or from other JavaScript. In a method, `this` is the object, as it is in TiberScript. A TiberScript list arrives as a JavaScript array, a `data` value as an object, a number as a number, and what the function returns comes back as itself: an array is an ordinary TiberScript list, with `.push()` and the rest, and nothing at all is `null`. When the JavaScript stops on a mistake, the game says which function, where, and why: `The javascript function 'heal' at line 9 stopped: undefinedThing is not defined`, and `attempt` and `recover` catch it like any other error.

**A block that names what it sees.** A block in a function can also say exactly which values it is given, in brackets after the word. It sees those and nothing else of the function's:

```tiberscript
shareScore(score) = function
  javascript(score)
    if (navigator.share) navigator.share({ text: "I scored " + score + " in Dungeon of Tiber!" })
  end
end
```

The names in brackets are the TiberScript values the block can see, and the block can `return` one value back, straight into a variable:

```tiberscript
sumOf(nums) = function
  local total = javascript(nums)
    return nums.reduce((a, b) => a + b, 0)
  end
  return total
end

dayOfWeek() = function
  local day = javascript()
    return new Date().getDay()
  end
  return day
end
```

Inside is ordinary JavaScript, exactly as a web page would write it, with the whole browser in reach: `navigator`, `fetch`, `localStorage`, any library the page has loaded. A TiberScript function named in the brackets is a JavaScript function inside, so `javascript(double) return [1, 2, 3].map(double) end` calls TiberScript from JavaScript. A block whose JavaScript has a mistake is reported with its line as soon as the file loads, for example `The javascript block at line 3 does not load: Unexpected token`.

**The page, from TiberScript.** `js` is the page itself, reachable from plain TiberScript with no block at all:

```tiberscript
init() = function
  day = js.new("Date", 2024, 1, 29)
  date = day.getDate()
  seen = new js.Map()
  seen.set("gold", 5)
  biggest = js.Math.max(3, 9, 4)
  rounded = js.call("Math.round", 2.6)
  text = js.json(data(name = "Ada", hp = [3, 2, 1]))
  back = js.parse(text)
  kind = js.typeof(js.window)
end
```

Any name of the page follows `js.` directly (`js.Math`, `js.Date`, `js.localStorage`), a JavaScript object's fields are read with a dot and its methods called with one, and `new js.Map()` makes a JavaScript object the way `new` makes a TiberScript one.

| Call | What it does |
| --- | --- |
| `js.window`, `js.global` | the page itself |
| `js.new(class, ...)` | a JavaScript object: `js.new("Date")`, `js.new(js.Map)` |
| `js.call("Math.round", ...)`, `js.call(fn, ...)` | calls a JavaScript function by its name or as a value |
| `js.get(obj, key)`, `js.set(obj, key, value)`, `js.has(obj, key)` | any field, whatever its name, spaces and dashes included |
| `js.typeof(value)` | `"number"`, `"string"`, `"array"`, `"object"`, `"function"` or `"null"` |
| `js.eval(code)` | runs JavaScript text, and gives its value |
| `js.json(value)`, `js.parse(text)` | a value as JSON text, and JSON text as a value |
| `js.fetch(address, "text" / "json" / "bytes")` | a file from the web, as a promise |
| `js.import(address)`, `js.load(address)` | a JavaScript module, or a script for the page, as a promise |
| `js.then(promise) do (value, error)` | runs the block on the tick after the promise settles |
| `js.state(promise)`, `js.value(promise)`, `js.error(promise)` | `"waiting"`, `"done"` or `"failed"`, what it gave, and why it failed |
| `js.callback(fn)` | a TiberScript function JavaScript may call at any time |
| `js.listen(target, event) do (e)` | the page's events, with a handle whose `.stop()` stops listening |

**Waiting for JavaScript.** The web answers later: a file arrives, a module loads, a timer fires. A promise is a value like any other, and a sequence can wait for it:

```tiberscript
loadLevel(name) = function
  sequence do
    local level = wait(js.fetch(name + ".json", "json"))
    if level != null then print("loaded " + level.title) end
  end
  js.then(js.fetch(name + ".txt")) do (text, error)
    if error != null then print("no notes: " + error) end
  end
end
```

`wait(promise)` pauses the sequence until the promise settles and gives its value, or `null` if it failed; `js.then(promise) do (value, error)` runs a block once it has, with the reason in `error` when it failed (`"HTTP 404"` for a file that is not there).

**Callbacks, both ways.** A TiberScript function given to JavaScript inside a call, like the `map` above, runs right there. JavaScript that calls back later (a timer, a page event, a library's message) goes through `js.callback(fn)` or `js.listen()`, and its calls are kept and run at the start of the next tick, never in the middle of one, so the game's world only ever changes on a tick:

```tiberscript
watchKeys() = function
  lastKey = "none"
  keys = js.listen(js.window, "keydown") do (e)
    lastKey = e.key
  end
  js.window.setTimeout(js.callback(function()
    print("half a second later")
  end), 500)
end
```

**What stays safe.** JavaScript runs as it is, so the Forge cannot check it for mistakes, and the Timeline cannot rewind what it does outside the game: a rewind, a forecast and a replay leave the page's own objects (the window, a `Date`, a `Map`) as they are, instead of copying them. The checker warns if a `javascript` block appears in `fixedUpdate()`. Use JavaScript for what lives outside the world, sharing, the clock, the web, a library, and bring its results into the world through a callback, which lands on a tick like any input.

**What it costs.** A `javascript` function or block is turned into a JavaScript function once, when the file loads, and called directly, so it costs what any other function call costs. A block at the top of a file costs nothing at all once the file has loaded: its functions are ordinary JavaScript functions, called straight.

**WebAssembly, in one word.** JavaScript is one of the two languages the Forge writes for the processor. The other is WebAssembly. The Forge makes TiberScript as fast as JavaScript, and for most of a game that is as fast as anything in a browser can be. For the few places a game does the same arithmetic on thousands of numbers, every frame, there is one more step, and it takes one word.

#### 19.20 fast: WebAssembly

```tiberscript
fast wrap(v, lo, hi) = function
  local span = hi - lo
  return lo + (v - lo) % span
end

fast falloff(d, radius) = function
  local t = clamp(1 - d / radius, 0, 1)
  return t * t * (3 - 2 * t)
end
```

A `fast` function is compiled to **WebAssembly**, the browser's second language: code that is already machine instructions when it arrives, laid out in one flat block of memory, with nothing for the browser to guess about. You call it like any other function, and it gives back the same values it would have without the word.

`fast` code works on numbers. It can use:

- numbers and `true`/`false`, `local`, `if`, `while`, `for` and `repeat`,
- math (`abs`, `sqrt`, `floor`, `ceil`, `round`, `sign`, `min`, `max`, `clamp`, `lerp`, `approach`, `remap`, `wrap`, `distance`, `angle`, `wave`, `sin`, `cos`, `tan`, `atan`, `atan2`, `exp`, `log`, `pow`), `noise`, and the seeded random stream (`random`, `randomFloat`),
- a value chosen by a condition (`1 if s.hp > 0 else 0`),
- other `fast` functions, itself included,
- functions written with `javascript` (`javascript name(n) = function`), which take numbers and give one back,
- buffers, and lists of numbers, read and written by their place: `b[i]` and `b.length` (19.23),
- the fields of a swarm's members, and `despawn()` (19.21).

It cannot draw, play a sound, make an object or a list, join strings, or call a TiberScript function that is not `fast`, and it has no `match`, `otherwise` or `in`: numbers are never `null`, and there is no list to look in. The checker holds the line and names it: "`fast` code cannot draw. `screen.circle` at line 12 is in `fast spark()`; draw from `update()` instead." Nothing slows down in silence. Take the word away, and the same code runs as ordinary Forge code.

#### 19.21 Fast Crowds

A swarm's `each` is where `fast` matters most:

```tiberscript
sparks = swarm of spark

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

A swarm already keeps its members as columns of numbers (19.10). `each fast` copies the columns its block uses into WebAssembly's memory, runs the whole loop there, and copies back the ones it changed, then removes the members it despawned. On 100,000 members that is about 0.9 ms against about 4 ms for the same `each` without the word. The rules of 19.15 already make a swarm's `each` numbers only, so most of them can simply add the word. Writing the loop with **vector instructions**, four members at every step, is the next step for `fast`.

An `each` without `fast` is still compiled to one tight JavaScript loop. `fast` is a promise the checker keeps: if the body ever stops being numbers only, it says so at the line, instead of quietly running slower.

#### 19.22 The Same Answer on Every Machine

Part One depends on one rule: the same world and the same inputs give the same result. On one machine and one browser, that is always true. Between machines it nearly is. Adding, multiplying and dividing give the same bits everywhere, but `sin`, `cos`, `exp` and the rest do not: each browser brings its own, and the last digit can differ. That is enough for a replay recorded on a laptop to drift on a phone, and for a rollback game between Chrome and Safari to disagree.

Inside `fast` code TiberScript uses its own math: `sin`, `cos`, `tan`, `atan`, `atan2`, `exp`, `log` and `pow`, written once with nothing but adding, multiplying and dividing (and `sqrt`, which is exact everywhere already), so they are the same to the last bit on every processor and every browser, and agree with the browser's own to about one part in a million billion. A replay is a replay everywhere. A rollback match between a phone and a laptop agrees on every tick. A seed shared online is the same level for everyone. And a run recorded on your laptop, a test that replays it included, replays exactly on a friend's. Outside `fast` code the math is still the browser's own (19.17), which can differ in the last bit between browsers; moving all of TiberScript's math to its own is the next step, and until then a game that must agree between machines keeps its trigonometry and powers in `fast` functions.

#### 19.23 Buffers: Numbers Every Language Shares

`fast` is one word. A buffer is what lets the three languages work on the same thing.

```tiberscript
motes = buffer(4000)

fast stepMotes(m, count, halfWidth, halfHeight) = function
  for i = 0 to count - 1 do
    local k = i * 4
    m[k] += m[k + 2]
    m[k + 1] += m[k + 3]
    if m[k] > halfWidth or m[k] < -halfWidth then m[k + 2] = -m[k + 2] end
    if m[k + 1] > halfHeight or m[k + 1] < -halfHeight then m[k + 3] = -m[k + 3] end
  end
  return count
end

javascript
  function quickest(list, count) {
    let best = 0
    for (let i = 0; i < count; i++) {
      const speed = Math.abs(list[i * 4 + 2]) + Math.abs(list[i * 4 + 3])
      if (speed > best) best = speed
    }
    return best
  }
end

init() = function
  for i = 0 to 999 do
    motes[i * 4] = randomFloat(-150, 150)
    motes[i * 4 + 1] = randomFloat(-80, 80)
    motes[i * 4 + 2] = randomFloat(-2, 2)
    motes[i * 4 + 3] = randomFloat(-2, 2)
  end
end

fixedUpdate() = function
  stepMotes(motes, 1000, 170, 95)
  top = quickest(motes, 1000)
end

draw() = function
  screen.clear()
  for i = 0 to 999 do
    screen.fillRect(motes[i * 4], motes[i * 4 + 1], 1.5, 1.5, "#8CC0FF")
  end
end
```

A thousand motes, four numbers each, in one buffer. TiberScript fills it and draws from it, a `fast` function moves it as WebAssembly, and a function written in JavaScript reads the quickest one out of it. All three are looking at the same numbers in the same place.

`buffer(n)` is `n` numbers, all 0 to start. `buffer(n, value)` starts them all at a value, and `buffer(list)` starts with a list's numbers. A buffer reads and writes like a list, `b[i]` and `b.length`, and has a list's ways of looking: `for v in b`, `.join()`, `.indexOf()`, and `.fill(value)` to set every number. Two things make it a buffer and not a list. It holds numbers and nothing else, and it never grows: a number written past its end goes nowhere, and a read past its end is `null`. `type(b)` is `"buffer"`, and it prints as the numbers it holds.

What that buys:

- **`fast` code works on it where it is.** A buffer lives in the memory WebAssembly runs in, so a `fast` function that is handed one reads and writes it directly, with nothing copied in and nothing copied back. The Forge knows a parameter is a buffer by how the function uses it, `m[i]` or `m.length`. Inside `fast` code a read outside the buffer is `0` and a write outside it is left out, so a neighbour that is not there (`src[i - 1]` at the edge of a grid) needs no `if`.
- **JavaScript sees the same memory.** A buffer handed to a `javascript` function or read in a `javascript` block is a typed array of numbers there, the same one, so a library written in JavaScript can fill it or read it.
- **A shader reads it.** A buffer given to a shader is a list of numbers on the graphics chip (19.37): the lights of a scene, the heights of a terrain, a field of distances.
- **It is part of the world.** The Timeline keeps a buffer's numbers and a rewind puts them back, into the same buffer. A forecast sees it as it will be and leaves it as it is, and `save.game()` saves it with everything else.

A `fast` function takes an ordinary list in a buffer's place too. The list is copied in for the call and copied back after it, which is right for a small list and wasteful for a big one: what is big, and changes every tick, belongs in a buffer.

Every buffer of a game shares one block of fast memory, 16 MB unless the game says otherwise, which is two million numbers. `Tiber.forge.memory = 64` at the start of `init()`, before the first buffer is made, gives it more. A buffer nothing holds any longer gives its room back, and one that does not fit stops with a message that says how much there is and how to give more. On a browser with no WebAssembly (`wasm.available` is `false`), a buffer is still a block of numbers that works the same way.

Measured on 100,000 motes, the loop above: 0.63 ms as WebAssembly, 0.70 ms as the JavaScript the Forge writes for the same `fast` function, 0.74 ms without the word `fast`, and 1.2 ms when the numbers are kept in an ordinary list instead. The buffer is most of the gain, and that is the plain truth of it: a browser's JavaScript is very good at a simple loop over numbers that sit side by side. WebAssembly adds the rest, a little here and more on heavy arithmetic, and it adds what JavaScript cannot: the same answer on every machine (19.22).

A swarm (Part Two) is for things, each with its fields, born and gone. A buffer is for a field of numbers with a fixed size: a grid of heights, a map of distances that a flood fill writes and a crowd reads, a block of samples, a table worked out once.

#### 19.24 wasm: WebAssembly in Your Own Hands

The Forge writes WebAssembly for what is marked `fast`. `wasm` hands that WebAssembly to the game, and lets a game bring WebAssembly of its own.

```tiberscript
fast cube(x) = function
  return x * x * x
end

init() = function
  bytes = wasm.bytes(cube)
  parts = wasm.describe(bytes)
  again = wasm.module(bytes)
  answer = again.run(3)
end
```

`wasm.bytes(fn)` is the module the Forge wrote for a `fast` function, as a list of bytes, the very file a WebAssembly tool would read (`null` for a function that is not `fast`). `wasm.describe(bytes)` says what a module gives and what it asks for before it is started: `parts.exports` and `parts.imports` are lists, each entry with a `name` and a `kind` (`"function"`, `"memory"`, `"global"` or `"table"`), and an import with the `module` it is asked from. `wasm.module(bytes)` starts a module at once, and its functions are then called like any others: `again.run(3)` is `27`.

A module does not have to come from the Forge. One compiled from C, Rust, Zig or AssemblyScript, or written by hand, joins a game the same way. These 41 bytes are a whole module with one function, `add`:

```tiberscript
init() = function
  adder = wasm.module([0, 97, 115, 109, 1, 0, 0, 0, 1, 7, 1, 96, 2, 124, 124, 1, 124, 3, 2, 1, 0, 7, 7, 1, 3, 97, 100, 100, 0, 0, 10, 9, 1, 7, 0, 32, 0, 32, 1, 160, 11])
  sum = adder.add(2, 40)
end
```

`sum` is `42`. The bytes can be a list of numbers as here, the bytes of a file (`js.fetch(address, "bytes")`), or the same bytes as base64 text.

| Call | What it does |
| --- | --- |
| `wasm.available` | `true` when this browser runs WebAssembly |
| `wasm.bytes(fn)` | the module the Forge wrote for a `fast` function, as a list of bytes |
| `wasm.describe(bytes)` | what a module gives and asks for: `exports` and `imports` |
| `wasm.module(bytes, imports)` | starts a module now, and gives what it exports |
| `wasm.load(address, imports)` | fetches a module and starts it: a promise, for `wait()` in a sequence or `js.then()` |

**What a module asks for.** The second value is what the module imports, as `data` inside `data`, the outer name being the module's and the inner one the function's: `wasm.module(bytes, data(env = data(log = onLog)))`. A TiberScript function given there is called by the module with its numbers, like any other call.

**What a module gives.** Each function it exports is a function of the value `wasm.module()` gives back. A global it exports reads as its value. And a memory it exports is a window onto the module's own numbers. For a module `m` that exports its memory under the name `memory`, `m.memory.numbers(at, count)` reads them as the numbers TiberScript uses, `.floats()` as shorter ones, `.ints()` as whole numbers and `.bytes()` as bytes, each starting `at` a byte and each live, so writing into one writes into the module. `.text(at, length)` reads text out of it, `.size` is how many bytes it has, and `.grow(pages)` gives it more.

Bytes that are no module stop with `wasm.module(): these bytes are not a WebAssembly module`, which `attempt` and `recover` catch like any other error.

What a module keeps in its own memory is outside the game's world, the way a JavaScript object is: the Timeline does not rewind it. A game that rewinds keeps its world in TiberScript values and buffers, and hands the module numbers to work on.

**The game's own memory, shared.** Every buffer (19.23) lives in one WebAssembly memory, and `wasm.memory` is that memory. Hand it to a module that takes its memory from outside, and the module reads and writes the game's buffers where they are, with nothing copied in or out: `wasm.module(bytes, data(env = data(memory = wasm.memory)))`. `wasm.address(buffer)` is where a buffer starts in it, in bytes (and -1 for a list, which is not in it), which is what such a module's functions take. `wasm.view(address, count)` goes the other way: `count` numbers of the fast memory, starting at an address, as a buffer TiberScript, JavaScript and `fast` code all read.

```tiberscript
init() = function
  heights = buffer([3, 1, 4, 1, 5])
  where = wasm.address(heights)
  same = wasm.view(where + 8, 2)
  same[0] = 100
  second = heights[1]
  megabytes = wasm.memory.megabytes
end
```

`second` is 100: `same` is not a copy of two numbers of `heights`, it is those two numbers. That is how a physics library, a pathfinder or a sound synthesizer written in another language and compiled to WebAssembly works on a game's own data: the game makes the buffers, the module is given the memory and their addresses, and both see every change the other makes at once.

#### 19.25 Tiber.forge: The Say Over the Forge

The Forge decides by itself what every function becomes. `Tiber.forge` is where a game can look at those decisions, measure them, and make a few of its own.

```tiberscript
fast cubeOf(x) = function
  return x * x * x
end

twice(n) = function
  return n * 2
end

init() = function
  kinds = [Tiber.forge.kind(cubeOf), Tiber.forge.kind(twice), Tiber.forge.kind("print")]
  written = Tiber.forge.javascript(twice)
  machine = Tiber.forge.wasm(cubeOf)
  oneCall = Tiber.forge.time(cubeOf, 1000, 7)
  waiting = Tiber.forge.compileAll()
  counts = Tiber.forge.stats()
end
```

`kinds` is `["webassembly", "javascript", "built in"]`. `written` is the JavaScript the Forge wrote for `twice()`, as text, and `machine` is the WebAssembly it wrote for `cubeOf()`, in WebAssembly's own text form, with `f64.mul` in it twice.

| Name | What it is |
| --- | --- |
| `Tiber.forge.kind(fn)` | what a function became: `"javascript"`, `"webassembly"`, `"shader"`, `"glsl"` (a shader written in GLSL), `"written in javascript"`, `"built in"`, or `"none"` for a value that is no function |
| `Tiber.forge.javascript(fn)` | the JavaScript the Forge wrote for it, as text |
| `Tiber.forge.wasm(fn)` | the WebAssembly it wrote for a `fast` function, as text (`null` for one that is not `fast`) |
| `Tiber.forge.glsl(shader)` | the GLSL it wrote for a shader |
| `Tiber.forge.time(fn, times, ...)` | calls a function that many times with the values that follow, and gives how long one call takes, in milliseconds |
| `Tiber.forge.compileAll()` | compiles every function nobody has called yet, now, and says how many there were |
| `Tiber.forge.stats()` | the counts: `files`, `functions`, `compiled`, `webassembly`, `shaders`, `javascriptBlocks`, `buffers`, `memoryUsed` and `memory` in megabytes |
| `Tiber.forge.useWasm` | `true` to start. `false` runs every `fast` function as the JavaScript the Forge also wrote for it |
| `Tiber.forge.memory` | the megabytes of fast memory buffers share (19.23) |

Three of them change what happens, and each has its use:

- **`Tiber.forge.compileAll()` on a loading screen.** The Forge compiles a function the first time it is called. For nearly every function that is too quick to notice, and for a big one it can be a millisecond in the middle of a fight. Called once while the title is on screen, it leaves nothing to compile later.
- **`Tiber.forge.useWasm = false` to compare.** The same game, the same answers to the last bit, with WebAssembly out of the picture: the way to see what `fast` buys on one computer, or to rule it out when hunting a mistake. `Tiber.forge.kind()` follows the switch.
- **`Tiber.forge.time()` instead of guessing.** Which of two ways to write a function is quicker is a question with an answer. It calls the function a few times first, so what is timed is the function and not the browser getting ready.

TiberScript, JavaScript and WebAssembly are not three ways to write a game, with a wall between them. They are one program: a TiberScript function calls a JavaScript one, a `fast` function calls a JavaScript one, JavaScript calls TiberScript back, a module from somewhere else calls TiberScript through its imports, and all of them read the same buffer. The Forge writes most of it, and where you want to write a piece yourself, it makes room.

**Where the time goes.** `Tiber.forge.profile = true` counts and times every call of every function from then on, and `Tiber.forge.hot(n)` is the `n` that took the most since: each a `data` with its `name`, the milliseconds it took in all (`ms`), how many times it was called (`calls`), the milliseconds one call takes (`each`), and what it is (`kind`). A function's time counts the functions it calls, so `update` is usually first and the one worth marking `fast` is the next one down. Counting costs a little, so it is for finding out, not for the finished game: `Tiber.forge.profile = false` stops it.

```tiberscript
init() = function
  Tiber.forge.profile = true
end

busy(n) = function
  local t = 0
  for i = 1 to n do
    t += sqrt(i)
  end
  return t
end

update() = function
  busy(2000)
end

draw() = function
  screen.clear("#101020")
  local y = 40
  for row in Tiber.forge.hot(3) do
    screen.text(row.name + "  " + round(row.ms, 1) + " ms in " + row.calls + " calls", 0, y, 7, "#C8C8DC")
    y -= 12
  end
end
```

**The code the browser keeps.** The Forge keeps what it compiles in the browser's storage, so the next Run of a game starts with nothing to compile. Three names rule that: `Tiber.forge.cache` is true to start, and false means nothing is read from the storage or written to it; `Tiber.forge.cacheUsed` is how much is kept there now, in megabytes; `Tiber.forge.clearCache()` drops all of it and says how many pieces there were. The browser's storage belongs to a game's saves before it belongs to the Forge: when it is full, compiled code is what gives way, and a save is always written.

#### 19.26 Measured

WebAssembly was measured against JavaScript before this part was built. The WebAssembly was written byte by byte, the way the Forge writes it, with no other tools.

| Workload | JavaScript | WebAssembly | WebAssembly with vector instructions |
| --- | --- | --- | --- |
| The bullet hell of 19.16, 10,000 bullets, a frame | 0.140 ms | 0.100 ms, 1.4x faster | the grid lookup cannot use them |
| The same, 100,000 bullets | 1.32 ms | 0.94 ms, 1.4x faster | |
| The same, 1,000,000 bullets | 17.9 ms | 14.1 ms, 1.3x faster | |
| Moving 4,000,000 particles, 50 times | 105 ms | 54 ms, 1.9x faster | 16.5 ms, **6.4x faster** |
| A snapshot of 100,000 bullets | 0.043 ms | 0.043 ms, the same | |

Every WebAssembly run gave exactly the same answer as the JavaScript one.

Plainly: on code that mixes arithmetic with looking things up, like the bullet hell's grid, WebAssembly is about 1.3 to 1.4 times faster than JavaScript that was already well written. On pure arithmetic over a crowd, which is what moving particles, sparks and swarms is, it is about twice as fast, and six times as fast with vector instructions. Snapshots are already as fast as memory can be copied, in both.

As shipped, a fast function gives exactly what the same function gives without the word, checked on thousands of calls, and `each fast` moves a swarm exactly as `each` does.

That is why `fast` is a word you add where it pays, and not something the Forge does to every line. For everything else, JavaScript is already the fastest thing the browser has, and the Forge's JavaScript comes within a few tens of percent of it. It is also why the win is measured and not promised: a crowd moving in about a fifth of the time is real today (19.21), a sixth with vector instructions was measured and is next, and nothing else in this handbook needed it until swarms made a million possible.

So the Forge writes three languages from one: JavaScript for everything, WebAssembly for what you mark `fast`, and GLSL for what you mark `shader` (Part Four). All three you can also write yourself: `javascript` (19.19), `glsl` (19.36), and WebAssembly brought in through `wasm` (19.24). Each meets TiberScript as an equal: the same values, the same calls, the same messages with the same line numbers, and `Tiber.forge` (19.25) to see what became of every function.

### Part Four: The Graphics Chip

Every computer, phone and tablet that runs a browser has two processors in it. One runs your game's code, one line after another. The other, the graphics chip, has hundreds or thousands of small cores that all run the same short program at once, one for every pixel of the screen. That is how a modern game draws an ocean that moves, a sky full of clouds no artist painted, a heat haze, a shockwave that bends the picture, or a black hole that pulls the stars around it. Those short programs are called **shaders**, and until now writing one has meant a second language (GLSL), a second set of rules, a page of setup code for WebGL, and no way to test it but to look at the screen.

In TiberScript 1.2 a shader is a TiberScript function with one word in front of it. The Forge reads it, checks it, and writes it twice: once in GLSL for the graphics chip, and once in JavaScript for the processor, so the same shader runs in a test, on a computer with no graphics chip at all, and inside your own code, and gives the same colors everywhere.

#### 19.27 shader

```tiberscript
shader sky(u, v) = function
  local low = color(0.95, 0.55, 0.3)
  local high = color(0.1, 0.15, 0.45)
  return mix(low, high, v)
end

draw() = function
  screen.clear("#000000")
  screen.shade(sky, 0, 0, screen.width, screen.height)
  screen.text("EVENING", 0, 0, 14, "#FFFFFF")
end
```

`shader name(u, v) = function ... end` is a function the graphics chip runs once for every pixel it is asked to fill. Its first two parameters are where that pixel is: `u` goes from 0 at the left edge to 1 at the right, and `v` from 0 at the bottom to 1 at the top, whatever the size of the area and the screen. What it returns is the color of that pixel: a `color(r, g, b)` or `color(r, g, b, a)` with every part from 0 to 1, or a plain number, which is a shade of grey (0 black, 1 white).

`screen.shade(shader, x, y, width, height)` fills a rectangle of the world with it, centered on `x` and `y` like `screen.fillRect()`, moved by the camera and a shake like everything else, and drawn over what is already there (a color with an alpha under 1 lets it show through). A sky, a lake, a force field, a portal, a health bar that shimmers: a shader is a way to paint an area, and it can be any size.

The sky above is two colors mixed from the bottom of the screen to the top. On a screen of 1920 by 1080 pixels that is two million pixels, every frame, and on a recent laptop's graphics chip it takes about a millisecond, most of it the browser handing the picture back.

#### 19.28 What a Shader Can Say

A shader runs on a different kind of processor, thousands of times at once, and has nothing but its own parameters to work with. That is why it is fast, and it is also what it cannot do. A shader can use:

- **numbers**, **true and false**, and **colors**, and `local` variables of each. A color's parts are `.r`, `.g`, `.b` and `.a`, and can be read and changed (`c.r = 1`).
- **arithmetic** between numbers and colors (`+`, `-`, `*`, `/`, and `%`, which is never negative for a positive divisor): `c * 0.5` is half as bright, `c + 0.2` is lighter, `a + b` adds two colors part by part. Comparisons (`<`, `>`, `==`, `!=` and the rest), `and`, `or` and `not`.
- `if`, `elseif` and `else`, `for i = 1 to n do`, `while`, `repeat(n) do`, `break` and `continue`, and `return`.
- `PI`, and these: `sin`, `cos`, `tan`, `asin`, `acos`, `atan`, `atan2`, `sqrt`, `abs`, `floor`, `ceil`, `round`, `fract` (the part after the point), `sign`, `exp`, `log`, `pow`, `min`, `max`, `clamp`, `step`, `smoothstep`, `mix` and `lerp` (of numbers or colors), `distance(x1, y1, x2, y2)`, `length(x, y)`, `angle(x1, y1, x2, y2)`, `wave(t, amount)`, `noise(x, y, z)`, `color()` and `rgb()`, `gray(n)`, `hsl(hue, saturation, lightness)` (hue in degrees), and `pixel(u, v)` (19.30).
- **other shaders**, which run as part of it (19.32).
- **what 19.34 and 19.37 add**: sprites, the frame before, richer noise, lists of numbers the game hands it, shapes by their distance, and a few ways to adjust a color.

And a shader cannot use anything else. The Forge checks every shader when the game loads, and says what is wrong and on which line, in words, before the game starts:

- `random()`: a shader is run for every pixel of every frame, and a random number would make the picture boil. `noise()` is the same every time for the same place (19.31). The message says so: `it cannot use random() (use noise(), which is the same every frame)`.
- **text**, **lists made inside it**, **objects**, **functions made inside it**, `this` and `null`: the graphics chip has numbers and nothing else. A list or a buffer the game hands it as a parameter is read a number at a time (19.37).
- **the game's own variables**, `screen`, `mouse` and every global: `A shader reads its own parameters and locals: 'speed'; pass it in`. A shader is given what it needs (19.29).
- **calls** to anything that does something (`print()`, `sfx()`, `emit()`, `shake()` and the like) or to any object's methods (only `math.` is allowed).
- **calling itself**, directly or through another shader: the graphics chip has no stack.
- **returning two kinds of thing**: every `return` of a shader gives a color, or every one gives a number.

A loop that would never end (`while true do`) is stopped after 100,000 turns, because a graphics chip that is stuck cannot be stopped from outside, and a frozen chip freezes the whole computer's screen, not only the game.

#### 19.29 Moving: a Shader's Own Parameters

```tiberscript
shader plasma(u, v, t) = function
  local a = sin(u * 10 + t)
  local b = sin(v * 8 - t * 1.3)
  local c = sin((u + v) * 6 + t * 0.7)
  return hsl((a + b + c) * 60 + t * 20, 0.8, 0.5)
end

init() = function
  clock = 0
end

update() = function
  clock += 1
end

draw() = function
  screen.shade(plasma, 0, 0, screen.width, screen.height, clock / 30)
end
```

Every parameter after `u` and `v` is the game's to give, as numbers, after the rectangle: `screen.shade(plasma, x, y, w, h, clock / 30)` gives `t`. A shader can have up to 16 of them: a time to move with, where a light or a player is, a strength, a color's parts. They are the same for every pixel of one call, and the graphics chip is given them fresh every call, so changing them every frame costs nothing.

That is the whole way a shader knows about the game, and it is also why a shader is always right: the picture it makes is worked out from its parameters alone. Rewind the game and pass the rewound `clock`, and the plasma is back where it was (19.38).

#### 19.30 The Whole Screen: screen.shader and pixel()

```tiberscript
shader underwater(u, v, t) = function
  local bend = sin(v * 40 + t) * 0.004
  local under = pixel(u + bend, v)
  return mix(under, color(0.1, 0.45, 0.6), 0.3)
end

init() = function
  clock = 0
  hero = register("hero", 0, 0)
end

update() = function
  clock += 1
  hero.x = mouse.x
end

draw() = function
  screen.clear("#203040")
  screen.fillRect(0, -60, 300, 30, "#C8B080")
  screen.fillCircle(hero.x, hero.y, 10, "#FFD050")
  screen.shader(underwater, clock / 10)
end
```

`screen.shader(shader, ...)` runs a shader over the whole screen and puts what it returns in place of the frame. On its own that is a full screen background. With `pixel(u, v)` it is a filter over everything already drawn: `pixel(u, v)` is the color of the frame at that place, as it was when `screen.shader()` was called. Ask for the pixel a little to the side and the picture bends, like water, heat haze or a lens; ask for the same pixel and change it, and it is a color grade:

- the underwater above, waving the picture and turning it blue green,
- a shockwave where a bomb went off: `pixel()` read from a ring that grows,
- the whole screen grey and slowly losing color as the hero's health runs out,
- a black hole bending the stars behind it (`screen.shade()` reads `pixel()` too, for the area it fills),
- night vision, a thermal camera, an old photograph, a dream.

The extra parameters work as in `screen.shade()`. What is drawn after `screen.shader()`, the HUD for example, is drawn over its result and is not changed by it. A shader that reads `pixel()` costs a little more than one that does not, because the frame has to be handed to the graphics chip first: on a recent laptop, a whole frame of 1920 by 1080 pixels read and changed takes between 1.3 and 3.2 ms.

#### 19.31 noise(): Clouds, Fire and Water Nobody Painted

```tiberscript
shader clouds(u, v, t) = function
  local n = 0
  local amount = 0.5
  local size = 3
  for i = 1 to 5 do
    n += noise(u * size + t, v * size, i) * amount
    amount *= 0.5
    size *= 2
  end
  local sky = color(0.35, 0.55, 0.9)
  return mix(sky, color(1, 1, 1), smoothstep(0.0, 0.4, n))
end

init() = function
  clock = 0
end

update() = function
  clock += 1
end

draw() = function
  screen.shade(clouds, 0, 0, screen.width, screen.height, clock / 400)
end
```

`noise(x, y, z)` is a smooth random looking number from about minus 1 to 1: close places get close numbers, far places unrelated ones, and the same place always gets the same number. Added up at a few sizes, as the loop above does (each size twice as fine and half as strong), it is clouds; stretched upward it is fire; moved with time it is water, smoke or a nebula. `y` and `z` may be left out.

It is the same arithmetic on the graphics chip and on the processor, whole numbers mixed and shifted the same way on both, so a cloud is the same cloud in a test, on a computer without a graphics chip and on every graphics chip. Noise is how a game gets endless worlds, skies and effects with no picture to draw and nothing to download.

#### 19.32 Shaders That Call Shaders, and Run on the Processor Too

```tiberscript
shader stripes(u, v) = function
  return step(0.5, fract(u * 8))
end

shader flag(u, v) = function
  local s = stripes(u, v)
  return mix(color(0.8, 0.1, 0.2), color(1, 1, 1), s)
end

init() = function
  middle = flag(0.25, 0.5)
  print(middle.r)
end
```

A shader calls another shader like any function, and the Forge writes the one it calls into the same program for the graphics chip, first. That is how a big effect is built from small, named parts: a `wood()`, a `rust()`, a `glowing()` that every material in a game uses.

And a shader called from ordinary code is an ordinary function: it runs on the processor and returns its color as an object with `.r`, `.g`, `.b` and `.a` (or its number). That is not a curiosity. It means:

- **A shader can be tested.** `test "the flag is red at the left" do assert(flag(0.01, 0.5).r > 0.7) end` runs with the game's other tests, with no screen.
- **The game can read what it draws.** A lava shader called with the hero's position says whether the hero is standing in the hot part.
- **A computer with no graphics chip still shows the game.** Where WebGL 2 is missing or turned off, `screen.shade()` and `screen.shader()` run the same shader on the processor, at about 60,000 points of the area, stretched smooth to fill it: softer, and slower, but the same picture. The debug overlay's graphics chip line ends with `(on the processor)` when that is happening.

The tests of 1.2 check the two against each other: every shader in them is drawn by a real graphics chip, read back, and compared pixel by pixel with the same shader run on the processor, and they agree to within 0.2% of a color's brightness (the tests allow 2%).

#### 19.33 A Million Points at Once

```tiberscript
star = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.vy = randomFloat(0.2, 1.5)
  end
end

stars = swarm of star max 100000

init() = function
  for i = 1 to 20000 do
    stars.spawn(randomFloat(-200, 200), randomFloat(-120, 120))
  end
end

fixedUpdate() = function
  stars.each fast do (s)
    s.y -= s.vy
    if s.y < -120 then s.y += 240 end
  end
end

draw() = function
  screen.clear("#02030A")
  stars.drawPoints(1.5, "#C8D8FF")
end
```

`swarm.drawPoints(size, color)` hands every member of a swarm to the graphics chip at once, as one list of positions read straight from the swarm's columns, and the chip draws them all as round, soft edged points in one go. `size` is in the game's units like a circle's diameter. A starfield, rain, snow, sparks, dust, a galaxy, the particles of an explosion: a crowd that only has to be seen.

Measured on a recent laptop: a million points drawn this way take 5.1 ms of a frame. The same million drawn as circles, one by one, by the browser's ordinary drawing took 145 ms. That is 28 times faster, and the difference between a slideshow and a game. Part Two made a million things cheap to keep and `fast` cheap to move; this is what makes them cheap to see. Without WebGL 2 it draws the swarm's own circles instead.

#### 19.34 More to Read: Sprites, the Last Frame, and Richer Noise

```tiberscript
shader clouds(u, v, t) = function
  local f = fbm(u * 4 + t * 0.05, v * 3, 0.55, 5)
  return mix(color(0.05, 0.07, 0.2), color(0.55, 0.6, 0.85), f)
end

shader cells(u, v) = function
  local d = voronoi(u * 9, v * 6)
  return hsl(200 + hash(floor(u * 9), floor(v * 6)) * 80, 0.6, 0.1 + d * 0.35)
end

shader trail(u, v, cx, cy) = function
  local p = previous(u, v)
  local g = max(0, 1 - distance(u, v, cx, cy) * 20)
  return color(p.r * 0.94 + g * 0.3, p.g * 0.94 + g * 0.6, p.b * 0.95 + g, max(p.a * 0.94, g))
end

shader ghostly(u, v) = function
  local c = image("hero", u, v)
  return color(luma(c), luma(c), 1, c.a * 0.6)
end

draw() = function
  screen.clear()
  screen.shade(clouds, 0, 0, 200, 120, time.now)
  screen.shader(trail, 0.5, 0.5, blend = "add")
end
```

A shader reads more than its own parameters and the picture under it:

- **`image(name, u, v)`** is the color of one of the project's sprites at `u, v`, from 0 to 1 across it: a sprite recolored, dissolved, outlined or made into a ghost by a shader, as `ghostly()` does. A shader can read up to four sprites by name, and `image(u, v)` with no name reads the one its draw was given (19.37).
- **`previous(u, v)`** is what this same shader drew there the frame before. Fading it a little and adding something new each frame leaves trails behind a moving ship, smoke that drifts, ripples that spread, a picture that builds up over time. `trail()` above is the whole of a glowing trail. A rewind clears it, since the frame before a rewind is not the frame before in the game.
- **`fbm(x, y, gain, layers)`** is layered noise from 0 to 1: `noise()` added to itself at finer and finer sizes, `gain` (0.5 is usual) for how much each finer layer counts and `layers` for how many (up to eight). Clouds, fire, marble, terrain seen from above.
- **`voronoi(x, y)`** is the distance to the nearest of a field of scattered points, from 0 to about 1: cells, scales, cracked earth, crystals, the bubbles of a slime.
- **`hash(x, y)`** is a number from 0 to 1 that looks random and is the same for the same place, every time: a different shade for each tile, each star, each cell.
- **`luma(color)`** is how bright a color looks to the eye, from 0 to 1, and **`dot(x1, y1, x2, y2)`** is `x1 * x2 + y1 * y2`, for light that falls on a slope.

Each one has the same numbers on the processor as on the graphics chip, checked by the tests on both, so a test, a computer without a chip and a game agree. A shader that moves a swarm (Section 25) cannot read `image()`, `previous()` or `pixel()`: a member is not a picture.

#### 19.35 Blending and Opacity

`screen.shade()` and `screen.shader()` draw over what is there. Two values by name at the end of either say how:

```tiberscript
shader layers(u, v) = function
  return color(0.5 + 0.5 * fbm(u * 3, v * 3, 0.5, 4), voronoi(u * 6, v * 6), hash(u * 8, v * 8), 1)
end

draw() = function
  screen.clear("#203040")
  screen.shade(layers, 0, 0, 100, 100, blend = "multiply", opacity = 0.5)
end
```

`opacity` is from 0 to 1. `blend` is one of `"normal"`, `"add"` (light on light: glows, trails, fire, magic), `"multiply"` (darkening: shadows, a tint, stained glass), `"screen"` (brightening without going white so fast), `"overlay"` (more contrast), `"darken"`, `"lighten"` and `"difference"` (a flash that turns the picture inside out). Both work the same on the processor.

#### 19.36 A Shader Written in GLSL

Most shaders are best in TiberScript: the checker reads them, the processor can run them, and a test can call them. Sometimes a shader already exists in GLSL, the graphics chip's own language, from an effect a game wants to borrow, or does something the checker refuses. `glsl` in front of a function makes its body GLSL itself:

```tiberscript
glsl plasma(u, v, t) = function
  float a = sin(u * 11.0 + t) + sin(v * 9.0 - t * 1.3) + sin((u + v) * 7.0 + t * 0.7);
  return vec4(0.25 + 0.25 * sin(a), 0.1 + 0.1 * cos(a * 1.7), 0.45 + 0.3 * cos(a), 1.0);
end
```

It is drawn like any shader, `screen.shade(plasma, 0, 0, 200, 120, time.now / 60)`, with `blend` and `opacity` too. Its parameters are numbers (`float` in GLSL), `u` and `v` first as always, and it returns a `vec4`: red, green, blue and opacity, each from 0 to 1. Inside it, the same helpers a TiberScript shader has are there by name: `pixel()`, `previous()`, `noise()`, `fbm()`, `voronoi()`, `hash()`, `hsl()` and `luma()`. A mistake in it is reported with the shader's line when the graphics chip reads it.

What it gives up is the processor: GLSL only runs on the graphics chip, so on a computer without WebGL 2 a `glsl` shader draws nothing, and says so once.

#### 19.37 Lists, Shapes by Distance, and How Finely It Is Drawn

```tiberscript
lamps = buffer(12)

shader glow(u, v, points, count) = function
  local x = (u - 0.5) * aspect()
  local y = v - 0.5
  local light = 0
  for i = 0 to count - 1 do
    local d = sdCircle(x - points[i * 3], y - points[i * 3 + 1], points[i * 3 + 2])
    light += 0.004 / max(0.004, d * d)
  end
  return color(light * 0.9, light * 0.7, light * 0.3, min(1, light))
end

shader badge(u, v, turn) = function
  local x = (u - 0.5) * aspect()
  local y = v - 0.5
  local rx = rotateX(x, y, turn)
  local ry = rotateY(x, y, turn)
  local edge = smoothstep(0.01, 0, sdBox(rx, ry, 0.25, 0.12))
  local tile = checker(rx * 16, ry * 16)
  return color(0.2 + tile * 0.3, 0.5, 0.9, edge)
end

init() = function
  for i = 0 to 3 do
    lamps[i * 3] = -0.45 + i * 0.3
    lamps[i * 3 + 1] = 0
    lamps[i * 3 + 2] = 0.03
  end
end

draw() = function
  screen.clear("#05060C")
  screen.shade(glow, 0, 0, screen.width, screen.height, lamps, 4)
  screen.shade(badge, 0, 40, 120, 60, time.now / 40, quality = 2)
end
```

Four lamps in a row, each a soft glow, and a checkered badge that turns above them. The lamps are not four parameters each: they are one buffer.

**A shader takes lists.** A parameter a shader reads by its place, `points[i]`, or asks the length of, `points.length`, is a list, and the game gives a list of numbers or a buffer (19.23) in that place: `screen.shade(glow, 0, 0, w, h, lamps, 4)`. That is how a shader is told about any number of things: every lamp of a room, every ripple on a pond, the heights of a horizon, the way a crowd is walking. A shader takes up to four lists, of up to 65,536 numbers each. The chip is handed the numbers as they are at each draw, so a list that changes every frame needs nothing more, and a place outside a list reads `0`. A shader can pass a list on to another shader that reads it. The shaders that are not drawn into an area of their own, a filter (23.11) and one that moves a swarm (Section 25), take numbers only.

**Shapes by distance.** `sdCircle(x, y, radius)`, `sdBox(x, y, halfWidth, halfHeight)` and `sdLine(x, y, ax, ay, bx, by)` say how far a point is from a circle, from a box, and from the line between two points. For the circle and the box the distance is below zero inside, zero on the edge, and grows outside. A distance is the most useful number a shader can have: `smoothstep(0.01, 0, d)` is the shape itself with a soft edge, `abs(d) < 0.01` is its outline, `0.004 / (d * d)` is a glow around it, and the smaller of two distances is two shapes joined into one. `rotateX(x, y, angle)` and `rotateY(x, y, angle)` are a point turned around the middle, the angle in radians: a shape turns by turning the point it is asked about, as the badge does. `checker(x, y)` is 0 or 1 in squares, and `aspect()` is the area's width over its height, so `(u - 0.5) * aspect()` keeps a circle round in a wide area.

**Colors, adjusted.** `saturate(color, amount)` takes a color toward grey (0 is grey, 1 leaves it alone, above 1 is richer), `contrast(color, amount)` pushes it away from middle grey or toward it, and `blur(u, v, radius)` is the frame under the shader, as `pixel()` reads it, softened over a small square: frosted glass, a pause screen, a far background out of focus.

**The sprite a draw is given.** `image(u, v)` with no name is the sprite the draw names with `sprite = "hero"`, so one shader serves every sprite of a game: `screen.shade(ghostly, x, y, 32, 32, sprite = "hero")`.

**How finely.** A shader is worked out once for every pixel of its area. `quality`, given by name at the end of a draw, says otherwise for that draw. `quality = 0.5` works out a quarter of the pixels and stretches the result smooth, which no eye catches on clouds, glows and fog, and it costs a quarter. `quality = 2` works out four times as many and draws them down, for the cleanest edge a shape can have. It goes from 0.1 to 2.

`Tiber.shaderScale` is the quality of every draw that does not say its own: `Tiber.shaderScale = 0.5` from a settings menu, for a phone or an older laptop. `Tiber.shaderScale = "auto"` lets the engine choose. It starts at 1, steps down a quarter at a time, to 0.25 at the least, while the game draws fewer pictures a second than the screen and the browser show, steps back up when there is room again, and says so once in the console.

How many pixels the whole game has to begin with is the game's to say as well: `Tiber.resolution = 1440` draws it on a frame 1440 lines high, `"4k"` on one 2160 lines high, and `720` on a small one for an old computer (21.3).

#### 19.38 What It Costs, and Why It Belongs Here

Measured on a recent laptop, a whole frame of 1920 by 1080 pixels (two million pixels) filled by one shader on the graphics chip: a rainbow made with `hsl()` 1.1 ms, clouds of five layers of noise 0.7 to 1.7 ms, a whole frame bent through `pixel()` 1.3 to 3.2 ms, a million points 5.1 ms. A frame at 60 has 16.7 ms. A shader is compiled when the game loads, and kept. Compiling one for the chip can take the browser half a second or more, so every shader a game has is handed to the chip as soon as the game loads, compiled while the game plays, and drawn once, a single pixel out of sight, as soon as it is ready: the first time a black hole or a shockwave appears in the middle of a game, its frame does not wait. The debug overlay (`Tiber.showDebugOverlay(true)`) has a line for it: `graphics chip: 3 shaders, 20000 points, 1.40 ms`.

`Tiber.hasFeature("shader")` (or `"graphics chip"`) says the engine has Part Four, and an exported game takes the graphics chip's code with it only when it uses a shader, a `glsl` shader or `drawPoints`.

What makes it Part Four and not a library:

- **The Forge** writes it. A shader is not a string of another language inside your game (unless you want it to be, 19.36): it is TiberScript, read by the same reader, checked with the same messages and line numbers, and compiled by the same compiler, which now writes three languages: JavaScript for the game, WebAssembly for the code marked `fast`, and GLSL for the graphics chip. Nothing else turns one language into all three.
- **Time** comes for free. A shader keeps nothing: what it draws is worked out from its parameters, every frame, so a rewound, replayed, forecast or rolled back game shows exactly the right sky, the right ripple and the right black hole, and there is no state of the graphics chip to save or send.
- **Swarms** are columns of numbers, and `drawPoints` hands those columns to the graphics chip as they are.
- **It is the same everywhere.** The integer noise and the processor's copy of every shader make the picture the same in a test and on every machine, the way `fast` makes the math the same.

### Part Five: HTML, Powered by the Whole Forge

A game is more than its play. Around the play are pages: a title, a shop, a settings screen, a board of scores, a bag of items, a level editor. The web has a language made for exactly those, and Section 9 opens HTML pages over a game. Part Five makes HTML part of TiberScript itself. A page is written in the game's own file, every value in it is TiberScript that the page always shows as it is now, every click on it runs the game's own code, and the Forge compiles all of it with the rest of the game. A page stops being a second program the game talks to, and becomes part of the game.

#### 19.39 html: A Page in the Game's Own File

```tiberscript
html shop
  <div class="shop">
    <h1>Shop</h1>
    <p>Gold: $(gold)</p>
    <p show="gold < 5">Not enough gold for anything</p>
    <ul>
      <li each="item in items">
        $(item.name)
        <button do="buy(item)" disabled="$(gold < item.price)">$(item.price) gold</button>
      </li>
    </ul>
    <button do="closeShop()">Leave</button>
  </div>
end

buy(item) = function
  gold -= item.price
  owned.push(item.name)
end

closeShop() = function
  html.hide("shop")
end

init() = function
  gold = 12
  owned = []
  items = [data(name = "Ring", price = 5), data(name = "Sword", price = 20), data(name = "Bow", price = 8)]
  html.show("shop")
end
```

`html shop`, on a line of its own in a file's top level (not inside a function), opens a page named `shop`, and a line with only `end` on it closes it. Between the two is HTML exactly as the web writes it, tags, attributes, CSS and JavaScript, with five things added, and all five are TiberScript:

| In the page | What it is |
| --- | --- |
| `$(gold)` | a value of the game, shown as it is now, in a text or in an attribute |
| `do="buy(item)"` | code the game runs when the element is clicked (a form: when it is sent; an input, a list of choices or a text area: when it is changed) |
| `bind="volume"` | an input kept equal to a value of the game, both ways |
| `show="gold < 5"` | the element is there only while this is true |
| `each="item in items"` | the element made once for every item of a list, `item` being that item inside it |

The block makes a page like every page of Section 9: `html.show("shop")` shows it over the game and the page takes the mouse and the keys, `html.hud("shop")` shows it and leaves them to the game, and `html.hide("shop")` hides it. A click on the Ring's button runs `buy(item)` with the Ring, `gold` goes from 12 to 7, and by the next frame the page says 7, the Ring's button is still on and the Sword's and the Bow's are off, because their `disabled` is worked out again from the new gold. Nothing in the game tells the page anything. The page reads the game.

#### 19.40 $( ): Values That Stay Up to Date

`$( )` holds any TiberScript value: a name (`$(gold)`), a field (`$(player.name)`), a sum (`$(score * 10)`), a call (`$(round(volume * 100))`), a choice (`$('full' if hp == maxHp else 'hurt')`). In a text it is part of the text; in an attribute it is part of the attribute, so `class="bar $('low' if hp < 20 else '')"` gives the bar a second class while the hero is low. Inside an attribute, a text is written with single quotes, `'like this'`, since the attribute itself is in double quotes; TiberScript reads a text in either.

An attribute that is nothing but one `$( )` that is true or false is the attribute on or off: `disabled="$(gold < item.price)"`, `checked="$(musicOn)"`, `hidden="$(paused)"`. A `\$(` writes a `$(` as it is, for a page that has to show one.

While a page is shown, every value in it is worked out again after every update of the game, and only what changed since the page last heard reaches it, in one message. When nothing changed, nothing is sent. A page that is not shown is not worked out at all, and costs nothing.

#### 19.41 do, bind and show

```tiberscript
html settings
  <h2>Settings</h2>
  <label>Volume <input type="range" min="0" max="1" step="0.05" bind="volume"> $(round(volume * 100))%</label>
  <label><input type="checkbox" bind="musicOn"> Music</label>
  <label>Name <input type="text" bind="hero.name"></label>
  <select bind="difficulty">
    <option value="easy">Easy</option>
    <option value="hard">Hard</option>
  </select>
  <p show="difficulty == 'hard'">Enemies hit twice as hard.</p>
  <button do="resetAll()">Back to the start</button>
end

resetAll() = function
  volume = 0.8
  musicOn = true
  difficulty = "easy"
end

init() = function
  hero = data(name = "Ada")
  resetAll()
  html.show("settings")
end
```

`bind` names something the game can set: a name (`volume`), a field (`hero.name`), an item (`scores[0]`). The input shows the value, and when the player moves the slider, ticks the box, types a name or picks a choice, the value is set in the game, before its next update. It goes both ways: when the game sets `volume` itself, as `resetAll()` does here, the slider moves to it. A slider and a number field give a number, a checkbox gives true or false, a text field and a list of choices give their text.

`do` holds one or more statements of TiberScript, the same as a function's body: `do="buy(item)"`, `do="gold += 1"`, `do="resetAll()"`. On a button or any element it runs when the element is clicked, on a `<form>` when the form is sent (the page is not left), and on an input, a list of choices or a text area when its value is changed.

`show` is a condition: the element is there while it is true and gone while it is not, with no space left behind. A mistake in any of them is said the moment the page is shown, with the line of the block, and the game goes on.

#### 19.42 each: Lists

```tiberscript
html bag
  <style>
    .bag { position: absolute; left: 4%; top: 8%; padding: 10px; border-radius: 10px; background: rgba(10, 20, 50, 0.9); color: white }
    .rare { color: gold }
  </style>
  <div class="bag">
    <h3>Bag: $(bag.length) of $(bagSize)</h3>
    <div each="item in bag" class="$(item.rarity)">
      $(item.name) x$(item.count)
      <button do="useItem(item)" show="item.usable">Use</button>
      <button do="bag.remove(item)">Drop</button>
    </div>
    <p show="bag.length == 0">Empty</p>
  </div>
end

useItem(item) = function
  item.count -= 1
  if item.count <= 0 then bag.remove(item) end
end

init() = function
  bagSize = 8
  bag = [data(name = "Potion", count = 3, rarity = "common", usable = true), data(name = "Star", count = 1, rarity = "rare", usable = false)]
  html.hud("bag")
end
```

`each="item in bag"` makes the element once for every item of the list, and inside it `item` is that item: its values, its `show`, its `do` (Use runs `useItem()` with the potion it is next to). The game's own names stay what they are inside, so `bag` and `bagSize` are still the game's. When the list changes, an item added, taken away or changed, the page changes with it. An `each` can be inside another, a row of cells for every row of a board:

```tiberscript
html board
  <table>
    <tr each="row in cells">
      <td each="cell in row" class="$(cell)">$(cell)</td>
    </tr>
  </table>
end

init() = function
  cells = [["X", "O", ""], ["", "X", ""], ["O", "", "X"]]
  html.show("board")
end
```

#### 19.43 The Page's Own Style and Script

A `<style>` and a `<script>` in the block are written into the page as they are, `$(` and all, so the CSS and the JavaScript of the web work as they always do. Everything of Section 9 still works beside the five words: an element with `data-send` sends a text, and the page's own JavaScript has `game.send()` and `game.on()`. `Tiber.html` is the same `html`, for a game that keeps everything it uses under `Tiber`.

#### 19.44 Powered by the Whole Forge

- **The Forge reads it.** Every piece of TiberScript in a page, its values, its clicks, its binds, its conditions and its lists, is read by the same reader and compiled to JavaScript by the same compiler as the rest of the game. A mistake is said with the block's line, as any other: `In the html block 'shop': a $( has no ) to close it`. Saving the file while the game runs (19.18) makes the page again, and a page that was shown stays shown.
- **JavaScript is native to it.** The page is the web's own: its CSS, its JavaScript, its fonts, its forms. A `javascript` block (19.19) can make more pages with `html.page()`, and a page written as an `html` block can be shown, hidden and given texts by the same calls.
- **It keeps nothing.** A page shows what the game is, and keeps nothing of its own. So a rewound, replayed or forecast game (Part One) shows the page it had then, with no work at all, and when the past changes (Part Six) the page shows the new present at the next update. Online, each machine's page shows its own player's game.
- **Anything the game has can be on it.** A value in a page can be a swarm's size (Part Two), a number of a buffer or the answer of a `fast` function (Part Three), a time, a translation (`$(tr('Shop'))`), anything TiberScript can say.
- **It is cheap.** Measured on a recent laptop: a page with forty values and a list of a hundred items, three values each, takes 0.03 ms of an update to work out, and a page not shown takes nothing. An exported game carries this part of the engine only when one of its files has an `html` block or uses `Tiber.html`, and on a game's server, which has no page, the blocks are kept and shown nowhere.

### Part Six: Change the Past

Every other part of this section looks at the past. This one changes it.

A game that keeps time (19.1) has every input of every kept tick, and it has its code. Make a change at a tick that already happened, play those same inputs forward again from there, and the present becomes the one that change would have made. The wall you put up a second ago stopped the hero a second ago. The enemy you made slower never caught up. The jump you gave more height cleared the pit it fell into. None of it is a guess: it is the same game, played again from the same inputs, with one thing different.

#### 19.45 retro

```tiberscript
init() = function
  time.keep(seconds(10))
  x = 0
  wall = false
end

fixedUpdate() = function
  if key("right").held then x += 1 end
  if wall and x > 50 then x = 50 end
  if key("w").hit then
    retro seconds(1) do
      wall = true
    end
  end
end
```

`retro N do ... end` goes back N ticks, runs its block there, and plays every kept tick since again with the game's code as it is now. Hold right for a hundred ticks and press W: `x` was 100, and it is 50 now, as though the wall had stood there for the last second. `retro do ... end`, with no length, makes the change at the very start of the time kept, and a retro further back than time is kept goes back as far as it can. The rest of the tick a retro is written in runs in the new present, and that tick counts as not played yet, as after `time.rewind()` (19.2).

A retro can be anywhere the game runs code while it plays: in `fixedUpdate()` on a key, as above, in `update()`, or in a button's block. What its block changes is the world as it was then, so `player.x` inside it is where the player was N ticks ago.

#### 19.46 What Came Out Different

`time.change` is the last change to the past: `why` (`"retro"`, `"your change to main"`, `"the tunable speed"` or `"time.changePast()"`), `ticks` and `seconds` played again, `changed` (whether anything came out different at all), `firstDifference` (the first tick that came out different), `ago` (how many seconds before now that tick is), `ms` (how long playing them again took), `undone`, and `what`. It is a picture of the change, so it cannot be written to.

`time.changes()` gives what is different now as lines of text, the names of the world and the fields of the objects kept in them, up to twelve:

```text
x  100 → 50
wall  false → true
```

A change that changes nothing says so: `changed` is false, `firstDifference` is null, and `time.changes()` is empty.

#### 19.47 Undo, and Compare

`time.undoChange()` puts back what had happened before the last change, exactly, the ticks kept with it included, and gives true; with nothing to undo it gives false.

`time.showOldPast(true)` draws what had happened over the new present, faintly: the old world's `draw()` is played again onto a picture of its own, from where the camera is now so the two line up, without its clear and without what it draws in `view world` (a HUD), and laid over the frame at 40%. The ship that turned around is where it is now, and its faint twin is where it would have been. `time.showOldPast(false)` stops it, and with nothing to compare it shows nothing.

#### 19.48 Save the Code, and the Past Changes

This is 19.8 grown up. While time is kept, saving a file while the game runs plays all the time kept again with the new code: a function you fixed has always been fixed, and a name the file's top level gives a new value has had it all along. Moving a tunable's slider (Section 4), or `Tiber.setTunable()`, moves it in the past too. A card at the top of the game says what happened: THE PAST CHANGED, how many ticks were played again with what, how long ago the first difference is, the first lines of `time.changes()`, and three buttons, KEEP (the card goes), UNDO (`time.undoChange()`) and COMPARE (`time.showOldPast()`).

`time.changeThePast = false` turns this off: a saved change then works from now on only, the way it did before Part Six. `time.changePast()` does it from code, over all the time kept. `time.changingThePast` is true while kept ticks are being played again.

#### 19.49 The Rules, and What It Costs

- **It changes kept time.** Without `time.keep()`, a retro says so (`retro changes kept time: time.keep(seconds(10)) in init() first`) and changes nothing.
- **It plays the kept inputs again**, so it asks for the Rules of Time (19.9) and nothing more. What `update()`, a button or a sequence changed between ticks is kept with the ticks, so it happens again too.
- **Not in an online game.** Every machine would have to change its past at once, so a retro there changes nothing and says why.
- **Not inside a forecast, a `whatif` or another retro.** Those run in a world that is thrown away, and a retro there does nothing.
- **Not in `fast` or `shader` code.** A retro holds a block of the game's own code, which the Forge does not compile to WebAssembly or for the graphics chip, and it says so on the line.
- **Quiet, and safe.** Sounds stay silent while the past plays again, `timeless` values stay as they are now, and if the code stops with an error while the past is played again, nothing changes: the present stays as it was and the error is shown.

Measured on a recent laptop: in a small scene with a ship that flies, two seconds (120 ticks) played again in 5.4 ms and all 491 kept ticks in 20 ms; with 34 physics bodies, 367 ticks in 155 ms; with a swarm of 1,036 sparks, 608 ticks in 302 ms. A change made with a key fits in a frame, and a change over ten seconds of a heavy game is one short pause.

#### 19.50 Why Six Parts Are One

- **Time needs the Forge and `fast`.** A forecast runs a second of the game inside one frame. On 1.1 that is impossible, on the Forge it is a few milliseconds, and `fast` is what makes it the same second on every machine, so rollback and replays work between them.
- **Swarms need `fast`.** A swarm is columns of numbers, and columns of numbers are what WebAssembly is made for. A million members is a curiosity in JavaScript and a working frame with `fast`.
- **The Forge needs the other parts** to be worth building a compiler at all. 1.1's interpreter is simple, and was fine for a game that runs its logic once a frame. Everything in this section runs it many times a frame. And because what it writes is JavaScript, the rest of the web is part of the same game (19.19).
- **The Graphics Chip needs the other three.** A shader is TiberScript only because the Forge can read it and write it again in GLSL, and in JavaScript for the processor. It keeps nothing, so Time rewinds the picture with no work at all. And a million swarm members that Part Two keeps and `fast` moves are only worth having if they can be drawn: `drawPoints` hands Part Two's columns straight to the chip.
- **HTML needs the Forge, and gives the other parts a face.** A page is TiberScript only because the Forge reads its values, its clicks and its lists and compiles them with the rest of the game. It keeps nothing, so Time rewinds it with the game, and a swarm of a million, a buffer or the answer of a `fast` function is a value it can show.
- **Changing the Past needs all five.** It is Time's kept inputs played again, hundreds of ticks inside one frame, which only the Forge and `fast` make quick enough to do while you wait. A swarm is played again as the columns of numbers it is, a shader draws the new present with no work at all, since it keeps nothing of the old one, and every page shows that present at the next update.

#### 19.51 Why This Changes Everything

Rewind has been built before: Braid, Prince of Persia, a dozen puzzle games, each one by hand for one game. Rollback has been built before, by fighting game studios, in C++, over years. Entity component systems have been built before, and every one of them asks you to rewrite your game in their shape. Trajectory previews, replays, ghost races, kill cams and planning AI get built from scratch, in every engine, every time someone needs one.

Every one of those is the same two ideas. The game's past and future are data. The game's crowds are data. TiberScript 1.2 is the language where both are simply true, for every game, from the first line, for free, and the Forge and `fast` are what make them cheap enough to leave switched on.

A beginner's first platformer can have a rewind button, a sky that moves, made by a shader of a few lines, and a shop written as a web page in the same file, with no code to keep the two in step. A student's first shooter can have fifty thousand bullets and never stutter, and a million stars with one word, drawn in a few milliseconds. A player can send the exact run where the bug happened, and it plays the same on the developer's machine, as a test that proves the bug never comes back. And the language underneath all of it is still the one a beginner learns first, running at the speed of the browser's own code, and of its graphics chip.

And then the past stops being a record and becomes something you can work on. Programmers have dreamed for years of changing the code and watching the game's past change with it, and it has been shown in talks and built by hand for single demos. In TiberScript it is every game: fix the jump, save, and the run you are in becomes the run where the jump was always right; wonder what a wall would have done, and see it, then undo it. The past, the future and every crowd in them are data, and data can be changed.

That is the biggest update of all.

---

## 20. Quick Reference Card

1.1's full reference card still applies in full. This table covers what is new in 1.2 only.

| Category | Keyword / Syntax | Description |
| --- | --- | --- |
| **Time** | `time.keep()`, `time.keep(seconds(n), memory = mb, during = state)`, `time.rewind()`, `time.rewindTo(tick or name)`, `time.mark(name)`, `time.forget()`, `time.stop()`, `time.oldest`, `time.kept`, `time.atOldest`, `time.memory`, `time.paused`, `time.forecast()`, `forecast N each (w) do`, `whatif do`, `time.replay()`, `time.record()`, `time.saveRecording()`, `timeless` | Part One of the biggest update. Rewind as far as the whole game, forecast, what if, replays. |
| **Time at any speed** | `time.rewind(0.5)`, `time.forward(n)`, `time.ahead`, `time.ago(n)`, `time.speed`, `time.slow(speed, ticks, ease = )`, `time.freeze(ticks)`, `time.step(n)`, `time.resume()`, `time.slowed`, `time.frozen`, `time.smooth` | Part One. Going back at a part of a tick's speed, forward again over what was gone back over, a picture of the world a while ago, and the whole game slower or faster, drawn smoothly in between. |
| **Change the Past** | `retro N do ... end`, `retro do ... end`, `time.change`, `time.changes()`, `time.undoChange()`, `time.showOldPast(on)`, `time.changePast()`, `time.changeThePast`, `time.changingThePast` | Part Six, the biggest of them all. A change made in the past, and the present it makes. |
| **Rollback and online** | `online`, `online N`, `online.started`, `.waiting`, `.watching`, `.me`, `.players`, `.seats`, `.watchers`, `.room`, `.link`, `.status`, `network.rollback(players = N, delay = N)`, `action(name, player)`, `network.me`, `network.rollbacks` | Online play in one word: rooms, a lobby with a link and a QR code, watchers, and rollback netcode, no netcode written. |
| **Swarms** | `swarm of name`, `max N`, `.spawn()`, `.each do (m)`, `.each touching "tag" do (m, o)`, `.despawn()`, `.alive`, `.count`, `.clear()`, `.draw()` | Part Two. A crowd kept as blocks of numbers. |
| **where** | `for x in list where condition do` | Only the items that match, in lists and swarms. |
| **The Forge** | saving while the game runs | Part Three. Compiled to JavaScript, and changed live. |
| **JavaScript** | `javascript ... end` (a block of JavaScript in full, at the top of a file or in a function), `local v = javascript ... end`, `export javascript ... end`, `javascript name(args) = function`, a `javascript` method, `javascript(names) ... end`, `Tiber.call()`, `Tiber.wrap()`, `Tiber.make()`, `Tiber.globals`, `js.window`, `js.Name`, `new js.Name()`, `js.new()`, `js.call()`, `js.get()`, `js.set()`, `js.has()`, `js.typeof()`, `js.eval()`, `js.json()`, `js.parse()`, `js.fetch()`, `js.import()`, `js.load()`, `js.then(p) do (value, error)`, `js.state()`, `js.value()`, `js.error()`, `js.callback()`, `js.listen() do (e)`, `wait(promise)` | Part Three (19.19). JavaScript, native to TiberScript. |
| **Fast** | `fast name(args) = function`, `.each fast do (m)` | Part Three (19.20). Compiled to WebAssembly, the same answer on every machine. |
| **Buffers** | `buffer(n)`, `buffer(n, value)`, `buffer(list)`, `b[i]`, `b.length`, `b.fill(value)` | Part Three (19.23). A block of numbers that TiberScript, JavaScript, WebAssembly and shaders read in the same place. |
| **WebAssembly** | `wasm.available`, `wasm.bytes(fn)`, `wasm.describe(bytes)`, `wasm.module(bytes, imports)`, `wasm.load(address, imports)` | Part Three (19.24). The module the Forge wrote, and modules of your own. |
| **Shared memory** | `wasm.memory`, `wasm.address(buffer)`, `wasm.view(address, count)` | Part Three (19.24). The game's own fast memory given to a module made elsewhere, so it works on the game's buffers where they are. |
| **The Forge's say** | `Tiber.forge.kind(fn)`, `.javascript(fn)`, `.wasm(fn)`, `.glsl(shader)`, `.time(fn, times, ...)`, `.compileAll()`, `.stats()`, `.useWasm`, `.memory` | Part Three (19.25). What every function became, and a few choices of your own. |
| **Profile and cache** | `Tiber.forge.profile`, `Tiber.forge.hot(n)`, `Tiber.forge.cache`, `Tiber.forge.cacheUsed`, `Tiber.forge.clearCache()` | Part Three (19.25). Which functions take the time, and the compiled code the browser keeps. |
| **The Graphics Chip** | `shader name(u, v, ...) = function`, `glsl name(u, v, ...) = function`, `screen.shade(shader, x, y, w, h, ..., blend = , opacity = )`, `screen.shader(shader, ..., blend = , opacity = )`, `quality = `, `sprite = `, a parameter read as a list (`points[i]`, `points.length`), `pixel(u, v)`, `previous(u, v)`, `image(name, u, v)`, `image(u, v)`, `blur(u, v, radius)`, `noise(x, y, z)`, `fbm()`, `voronoi()`, `hash()`, `luma()`, `dot()`, `sdCircle()`, `sdBox()`, `sdLine()`, `rotateX()`, `rotateY()`, `checker()`, `aspect()`, `saturate()`, `contrast()`, `color()`, `gray()`, `hsl()`, `mix()`, `smoothstep()`, `fract()`, `Tiber.shaderScale`, `swarm.drawPoints(size, color)`, `swarm.moveOnChip(shader, ...)` (Section 25) | Part Four. TiberScript run on the graphics chip, once for every pixel. |
| **Light** | `lighting.ambient`, `.tint`, `.shadows`, `.glow`, `.quality`, `.walls`, `.softness`, `.block()`, `.blockLine()`, `.blockCircle()`, `.draw()`, `.at()`, `.colorAt()`, `.dark()`, `.off()`, `.count`, `.lastCount`, `screen.light(x, y, radius, color, strength, shadows = , flicker = )`, `screen.spotlight(x, y, radius, angle, width, color, strength)`, `screen.sunlight(angle, strength, color, length)`, `screen.glow(x, y, radius, color, strength)`, `swarm.lights(radius, color, strength)` | Section 23. Lights, sunlight, shadows, glow, and how bright a place is. |
| **Mirrors and water** | `lighting.mirror(x1, y1, x2, y2, strength)`, `lighting.bounced`, `screen.mirror(x, y, w, h, side, strength, tint = , fade = , ripple = , scale = , blur = )`, `screen.water(x, y, w, h, ...)` | Section 23.13. A mirror that throws light back, and rectangles that show the picture turned over, still or rippling. |
| **Effects** | `effects.crt`, `.bloom`, `.scanlines`, `.vignette`, `.chromatic`, `.grain`, `.pixelate`, `.tint`, `.tintAmount`, `.active`, `.off()`, `effects.filter(shader, ...)` (`.strength`, `.on`, `.set()`, `.remove()`), `effects.filters`, `effects.clearFilters()` | Section 23. A finish on the whole frame, and your own shaders stacked on it. |
| **Physics** | `physics.add(obj, "box", w, h)`, `physics.add(obj, "circle", r)`, `physics.add(tilemap)`, `fixed =`, `bounce =`, `friction =`, `mass =`, `gravity =`, `physics.push()`, `physics.touching()`, `physics.remove()`, `physics.gravity`, `.iterations`, `.paused`, `.count`, `.bodies()`, `body.contacts`, `body.grounded`, `body.angle` | Section 24. Bodies that rewind, forecast and roll back exactly. |
| **Modules** | `import "file"`, `export name = ...`, `export const NAME = value`, `export javascript ... end` | Real multiple file projects. An exported value is shared, not copied, and a module's own functions come first inside it. |
| **Tiber libraries** | `Tiber.create("tools")`, `Tiber.create = "tools"`, `Tiber.tools.double(2)`, `Tiber.libraries` | A module of yours kept on your computer, reached from every one of your projects (Section 3). |
| **Older versions** | `tiberscript 1.0 ... end`, `tiberscript 1.1 ... end`, `tiberscript 1.2 ... end`, `Tiber.version = "1.1"` on a line of its own | Code read by an older version's rules, as a block or as a whole file (Section 1). |
| **Strings** | `"$(value)"`, `\$` | Interpolation, and its escape. |
| **Functions** | `function(a, b) ... end`, `f(x, speed = 1)`, `f(y = 2, x = 1)` | Inline functions as values; parameters with values of their own, and values given by name. |
| **Choosing** | `match v`, `case 1, 2 then`, `case 3 to 9`, `else`, `value if condition else other`, `a otherwise b`, `x in list`, `x not in list` | One case of many, a value by a condition, a default for null, and membership. |
| **elseif** | `if ... then ... elseif ... then ... else ... end` | Another way to write `elsetry`: either one is fine (Section 1). |
| **Constants** | `const NAME = value` | A name that keeps its value. |
| **Loops** | `for` counters are local to their loop, `for name, value in data`, `for i, item in list` | The one change from 1.1, and loops over two names. |
| **Time units** | `seconds(n)` | Seconds anywhere frames go. |
| **string** | `string.join()`, `string.format()`, `.padLeft()` | On top of 1.1's string methods. |
| **array** | `array.filter()`, `array.sort()`, `array.find()`, `array.contains()`, `array.ofObjects()`, `array.shuffle()`, `array.sum()`, `array.average()`, `array.unique()`, `array.first()`, `array.last()` | Higher order helpers using functions as values, and everyday list math. |
| **Game feel** | `approach()`, `remap()`, `wrap()`, `round(n, digits)`, `ease(name, t)`, `cooldown(name, ticks)`, `cooldownLeft(name)`, `chance(p)`, `weightedChoice(items, weights)`, `grid(w, h, fill)`, `type()`, `inspect()`, `tr(key, ...)`, `Tiber.language` | The small math, odds, grids and translations every game writes again. |
| **Translate** | `Tiber.translate = "es"`, `Tiber.translate("japanese")`, `"auto"`, `"off"`, `Tiber.translated(text)`, `Tiber.translateFrom` | Every text the game draws in another language, in the same width, numbers kept (Section 2). |
| **vector / math** | `vector(x, y)`, `.length()`, `.normalize()`, `.add()`, `math.atan2()`, `math.lerp()` | Real vector math and interpolation. |
| **table** | `table.keys()`, `table.get()`, `table.set()`, `table.has()`, `table.remove()`, `table.copy()` | Dynamic key and value access for `data` blocks. |
| **Traits** | `trait ... end`, `archetype uses A and B` | Shared behavior without forced inheritance. |
| **Tunable values** | `export name = value` inside an archetype or `data` block | Live sliders in the run window, written back to the code on request. |
| **Enums** | `enum ... end` | Named, typo checkable constants. |
| **State hooks** | `entering state do`, `leaving state do` | Run once when a state starts or ends. |
| **Sequences** | `sequence do ... end`, `wait(N)`, `wait(seconds(n))`, `wait(signal)`, `wait(promise)`, `wait until condition` | Ordered, pausable steps. |
| **Quick events** | `on("name") do (data) ... end`, `trigger("name", data)` | Decoupled, global communication. |
| **Signals** | `signal name(args)`, `.trigger()`, `.connect()`, `.disconnect()` | Declared, typed communication scoped to one object. |
| **Controllers** | `gamepad(n).held()`, `.hit()`, `.released()`, `.amount()`, `.axis()`, `.angle()`, `.anyHit()`, `.button()`, `.label()`, `.drawButton()`, `.draw()`, `.rumble()`, `.stopRumble()`, `.connected`, `.kind`, `.name`, `.player`, `.model`, `.joycon`, `.setUp`, `.silent`, `.layout`, `.canRumble`, `.deadzone`, `controllers.count`, `.list`, `.first`, `.held()`, `.hit()`, `.anyHit()`, `.button()`, `.setup()`, `.setupKey`, `.forget()`, `.settingUp`, `.deadzone`, `.hint`, `.pairJoycons`, `.report()`, `.rawReport()`, `on("controllerConnected")`, `on("controllerDisconnected")` | Every controller, wired or wireless, by where its buttons are. |
| **Controllers read directly** | `controllers.direct`, `controllers.connect()`, `pad.direct`, `pad.pace`, `pad.motion`, `pad.battery`, `pad.setLight(color)` | Section 7. Nintendo's, Sony's and Microsoft's controllers read from their own reports the moment they arrive, by microStudio's bridge or by the browser: the controller's own pace, its sticks' calibration, the motion sensor, the battery, rumble and the player's light. |
| **Keyboards** | `key("primary")` (Command on a Mac, an iPhone and an iPad, Control elsewhere), `key("command")`, `key("option")`, Control Shift and a number for F1 to F10, `system.platform` | One keyboard for every computer. Section 7. |
| **The mouse** | `mouse.wheel` (1 down, -1 up, 0), `mouse.hit` (a click between two updates counts too) | Section 7. |
| **Held, tapped, repeated** | `pad.heldFor(name)`, `pad.repeat(name, delay, rate)`, `pad.tapped(name)`, `pad.longPress(name, ticks)`, `pad.doubleTap(name)`, the same on `controllers`, `key(name).heldFor`, `.repeat`, `.released`, `action(name).repeat`, `repeats(ticksHeld, delay, rate)` |
| **How far down counts** | `controllers.threshold`, `pad.threshold`, `pad.setThreshold(name, press, release)`, `controllers.stickThreshold`, `controllers.curve`, `pad.raw(axis)`, `pad.stick("left")`, `controllers.amount(name)`, `controllers.axis(name)`, `"lsUp"` to `"rsRight"`, the words (`"light"`, `"wide"`, `"aim"`...) and `"auto"`, `controllers.feel`, `pad.drift` |
| **More buttons** | `"extra"` (`"touchpad"`, `"capture"`, `"share"`), `"touch"`, `"mute"`, `"paddle1"` to `"paddle4"`, `"chat"`, `"sl"`, `"sr"`, `"cUp"` to `"cRight"`, `pad.has(name)`, `pad.touches`, `pad.setTrigger(name, strength, from)`, `controllers.inUse`, `controllers.active`, `controllers.rumble()` |
| **The settings screen** | F4, or START with SELECT; `Tiber.settings.open()`, `.close()`, `.isOpen`, `.next(name)`, `.set(name, value)`, `.get(name)`, `.name(name)`, `.changed`, `.reset()`, `.rumble`, `.key`, `.combo` |
| **Transitions** | `transition.fade(seconds) do ... end`, `transition.wipe(seconds, from)`, `transition.iris(seconds, x, y)`, `transition.blinds(seconds)`, `color = `, `back = `, `hold = `, `count = `, `transition.active`, `transition.phase`, `transition.covered`, `transition.kind`, `transition.stop()` | Section 5. The screen covered, the block run while it is, and uncovered. |
| **Colors** | `colors.mix(a, b, amount)`, `colors.lighten()`, `colors.darken()`, `colors.fade(color, alpha)`, `colors.rgb()`, `colors.hsl()`, `colors.parts()`, `colors.brightness()`, `colors.valid()` | Section 2. One color made from another. |
| **Springs and watch** | `springTo(object, "field", target, stiffness = , damping = )`, `watch(label, value)`, `Tiber.watch` | Sections 2 and 12. A number that reaches its target with a bounce, and values shown on the running game. |
| **Motion** | `Tiber.motion`, `Tiber.prefersLessMotion` | Section 13. How much the screen may shake and flash, and what the player's system asks for. |
| **Actions** | `bind("action", input)`, `action("name").hit` / `.held` / `.released` / `.amount` / `.heldFor` / `.within(n)` / `.consume()` / `.releasedWithin(n)`, `actionAxis()`, `actionVector()`, `touch.button()` | Input mapped to intent, rebindable, touch included, analog, and buffered. |
| **Animation** | `obj.animate(name, frames, speed)`, `.playAnimation(name)`, `.flip`, `screen.sprite("name.2", x, y, size)`, `screen.sprite(name, x, y, width, height)` with a negative width | Sprite frame animation, one frame by name, mirrored. |
| **Camera** | `camera.follow()`, `.bounds()`, `.layer()` | Smooth tracking, level bounds, parallax. |
| **Draw layers** | `screen.layer(n)` | Real control over render order. |
| **Fixed physics** | `fixedUpdate()` | Physics on a steady timestep, independent of frame rate. |
| **Physics** | `.mass`, `.friction` | Weight and drag per object. |
| **Tween easing** | `tween(..., ease = "bounce")` | Non linear animation curves. |
| **audio** | `audio.setVolume(channel, level)`, `audio.has(name)` | Separate volumes, and asking what a project contains. |
| **UI** | `ui.button()`, `ui.healthbar()`, `ui.textbox()` | Built in UI primitives. |
| **Dialogue** | `dialogue "text" speaker="Name" do`, `choice "text" do`, `Tiber.talking` | Branching conversation system. |
| **Collision** | `.collisionShape()`, `.collisionLayer()`, `.collidesWith()`, `resolve()` | Shapes, layers, and real resolution. |
| **Tile maps** | `tilemap(name, x = , y = )`, `.solid()`, `.oneWay()`, `.collide(obj)`, `.tileAt()`, `.set()`, `.onFloor` | Platformer collision against a map, done for you. |
| **Noise** | `noise(x)`, `noise(x, y)`, `noise(x, y, z)` | Smooth randomness for terrain, flicker and wandering. |
| **Pathfinding** | `path.find(map, from = , to = , diagonal = )` | Shortest walkable route, or `null`. |
| **Timers** | `schedule()` returns a handle, `cancelSchedule(handle)` | Timers you can call off. |
| **Randomness** | `seedRandom()`, `randomFloat()`, `randomBool()`, `randomChoice()`, `randomSign()` | Everything `random()` alone never covered. |
| **Testing** | `test "name" do`, `test "name" replay "run" do`, `assert(cond)` | On demand correctness checks, and tests that play. |
| **Frame rate** | `Tiber.frameRate = 60 / 120 / 240 / "auto"`, `Tiber.renderScale = 1 / 0.75 / 0.5 / "auto"`, `Tiber.resolution = 720 / 1080 / 1440 / "4k" / "native"`, `Tiber.frameWidth`, `Tiber.frameHeight`, `Tiber.fps`, `Tiber.smooth`, `Tiber.tickRate = 60 / 120 / 240 / "auto"`, `Tiber.tickScale`, `Tiber.dt`, the performance monitor's switches (F3) | Smooth on every computer, and 120 or 240 updates a second (Section 21). |
| **HTML blocks** | `html name ... end`, `$(value)`, `do="code"`, `bind="name"`, `show="condition"`, `each="item in list"`, `\$(`, `Tiber.html` | Part Five (19.39). A page written in the game's own file, its values always up to date. |
| **HTML pages** | `html.show(name)`, `html.hud(name)`, `html.hide(name)`, `html.send(name, text)`, `html.receive(name)`, `html.set(name, id, text)`, `html.page(name, text)`; in the page `game.send()`, `game.on()` and `data-send` | The project's own web pages over the game (Section 9). |
| **Platform** | `platform.define()`, `.unlock()`, `.unlocked()`, `.achievementList()`, `.clearAchievement()`, `.stat()`, `.setStat()`, `.addStat()`, `.submitScore()`, `.scores()`, `.refreshScores()`, `.cloudSave()`, `.cloudLoad()`, `.cloudDelete()`, `.user()`, `.setPlayerName()`, `.setPresence()`, `.openOverlay()`, `.showToasts()`, `.events()`, `.use()`, `.name`, `.ready` | Steam, Epic, consoles or the browser (Section 22). |
| **Save data** | `save.set()`, `save.get()`, `save.setObject()`, `save.getObject()`, `save.write()`, `save.list()`, `save.use()`, `save.delete()`, `save.game()`, `save.loadGame()`, `save.hasGame()`, `save.gameInfo()`, `save.deleteGame()` | Structured persistence, and a whole game saved in one line. |
| **Networking** | `network.send()`, `network.on("event") do (msg, from)` | Multiplayer hooks. |
| **Tiber** | `Tiber.extendArchetype()`, `Tiber.hasFeature()`, `Tiber.showDebugOverlay()`, `Tiber.runTests()`, `Tiber.tunables()`, `Tiber.setTunable()`, `Tiber.version`, `Tiber.forge` | Extending, checking, testing and inspecting the engine itself. |
| **Capture** | `capture.shot(name)`, `capture.moment(name)`, `capture.auto`, `capture.clips`, `capture.onAction(name)`, `capture.offAction(name)`, `capture.quiet`, `capture.on`, `capture.list`, `capture.last`, `capture.count`, `capture.save(item)`, `capture.clear()`, `on("captured")` | Section 26. Pictures and short clips of a game's best moments, taken by the game or found by the engine itself. |

---

## 21. Smooth on Every Computer: Frame Rate and the Hardware It Needs

### 21.1 Two numbers: pictures drawn, and updates

Turn on the developer overlay (F3) on a screen that refreshes 120 times a second, as many recent laptops, tablets and phones do, and it can say 120 while your game's own counter says 60. Both are right. The game updates 60 times a second: `fixedUpdate()` and `update()` run 60 times a second, `seconds(1)` is a second of them, and every speed in your game is written for that (a game can be written for 120 updates as well, 21.7). The screen may ask for a new picture 120 times a second. Before 1.2's frame rate setting, every other picture on such a screen was the same picture drawn again. The overlay's first line now says both: `120 fps   60 updates a second`. The next two say how often the monitor refreshes and what is in charge of each number: `monitor 120 Hz`, then `pictures GAME   updates 60`.

### 21.2 Tiber.frameRate

```tiberscript
init() = function
  hero = register("hero", 0, 0)
  Tiber.frameRate = "auto"
end

update() = function
  hero.x += 1
end

draw() = function
  screen.clear()
  screen.fillCircle(hero.x, hero.y, 8, "#8CC0FF")
end
```

`Tiber.frameRate` is how many pictures a second the game draws:

- **60**, the default. One picture after each update and none in between. On a 120 Hz screen this draws half as often as before, for the same game: less work, a cooler and quieter computer, longer battery.
- **120.** `draw()` also runs between two updates, with the world shown part of the way from the update before to the last one, then put back exactly as it was. Motion is twice as smooth, and the game plays at exactly the same speed: it still updates 60 times a second. What is shown part of the way: every `x` and `y` and every number whose name ends in x or y (a camera's `camX`), and every other number that is not a whole number, on every object, global and swarm member. A whole number that is not a position, like `hp`, `score` or a frame counter, is never shown halfway. A value `draw()` sets itself is kept. A forecast, a `whatif` or `save.game()` made inside `draw()` sees the real world.
- **240.** The same, with up to three pictures between two updates, for a screen that refreshes 240 times a second. On a slower screen it draws as many as the screen shows.
- **"auto"** draws as many pictures as the screen shows, up to 240 (120 on a screen under 200 Hz), while the game's work leaves room for it. It goes back to 60 when a frame's update and drawing take more than about three quarters of a frame on average (6 ms at 120, 3 ms at 240), and back up when they take less than about two fifths of one (3.5 ms at 120, 1.75 ms at 240).

The drawing has to be in `draw()` for 120 and 240: that is how the engine knows what is drawing and what is game. A game that draws in `update()` stays at 60, and says so once in the console. On a 60 Hz screen, 60, 120 and 240 look the same.

`Tiber.fps` is how many pictures a second the game is drawing right now, `Tiber.smooth` is true while it draws in between, and `Tiber.tickRate` is the updates a second, 60 unless the game asks for more (21.7). A click is one click and a flash lasts its updates, however many times the screen is drawn in between.

### 21.3 Tiber.renderScale

```tiberscript
init() = function
  Tiber.renderScale = 0.75
end
```

How many pixels the game draws, compared with the window: 1 is every pixel (the default), 0.75 is a little over half as many, 0.5 a quarter. The picture is stretched to the window, so the game looks the same, only softer. **"auto"** starts at 1 and steps down by a quarter (to 0.5 at the lowest) when, three seconds running, the game draws fewer pictures a second than the screen and the browser show, or its own work fills every frame, and says so once. A screen slower than the game asks for (a 60 Hz screen for a game set to 120 or "auto") and a browser that holds the page to 30 pictures a second to save the battery are not the game being slow, and keep every pixel. On a computer whose graphics cannot fill a big window in time, this is what keeps 60 frames a second. It goes from 0.25 up to 4: above 1 the game draws more pixels than the window has and the picture is drawn down to fit, which smooths every edge, for a computer with the graphics to spare.

**`Tiber.resolution`** says the same thing the way a game's settings menu does, as the height of the picture in lines:

```tiberscript
init() = function
  Tiber.resolution = "1440p"
end

draw() = function
  screen.clear()
  screen.text("$(Tiber.frameWidth) by $(Tiber.frameHeight)", 0, 0, 10, "#FFFFFF")
end
```

`Tiber.resolution = 1080` draws the game on a frame 1080 lines high whatever the size of the window, and keeps it there when the window changes: sharper than a small window needs, softer than a big one has, and always the same amount of work. It takes a number of lines from 120 to 4320, or a name: `"720p"`, `"1080p"`, `"1440p"` and `"4k"` (2160 lines), and `"native"` for the window's own pixels, which is where a game starts. So a game can offer 720p for a ten year old laptop, 1080p, 1440p and 4K in one menu, and the same line of code serves all four. A frame never has more pixels than a 4K picture, however big the window. `Tiber.resolution` reads back what was asked, and `Tiber.frameWidth` and `Tiber.frameHeight` are the pixels the frame really has right now.

The game's own units do not change with any of this. The screen is 200 units high at 720 lines and at 4K, so nothing in a game's code moves: only how finely it is drawn. Shaders have a setting of their own on top, `quality` and `Tiber.shaderScale` (19.37), because a shader's cost grows with every pixel it fills.

### 21.4 What it costs, measured

On a recent laptop, in a browser, a platform game of six thousand lines in its heaviest moment (a boss fight in a full arena):

| | Update a frame | Drawing a frame | Work in a second |
| --- | --- | --- | --- |
| `Tiber.frameRate = 60` | 0.06 ms | 0.14 ms | 12 ms of the 1000 |
| `Tiber.frameRate = 120` | 0.17 ms | 0.14 ms, twice | 27 ms of the 1000 |

That is the game's own work, in the browser's script engine. The rest of a frame is the browser turning the drawing into pixels, and that depends on the graphics chip and the size of the window. With the graphics chip doing it (the browser's hardware acceleration, on by default), filling a window is a small fraction of a frame on every computer in the table below. Without it, the browser fills every pixel on the main processor: the same frame then took 15 ms at 2560 by 1440 pixels. That is the case `Tiber.renderScale` and a lower `Tiber.resolution` are for.

### 21.5 The hardware it needs

The game's own work grows with how fast one core of the processor is. These are the computers TiberScript games run well on, for a full game: a platform game with bosses and crowds of enemies, a game for a phone, a scene of lights and shaders. One computer was measured. The others are worked out from it: each processor's single core speed against the measured one's, with the same game, and room to spare for the browser and the drawing. A browser means a current one with hardware acceleration on. Every export (Windows, Linux, macOS, Raspberry Pi) runs in the same browser engine, Chromium, and needs the same.

| Platform | 60 frames a second | 120 frames a second |
| --- | --- | --- |
| **Mac** | Any Mac from 2015 on (Intel Core i5 5th generation, Intel Iris 6100 or better), macOS 11 or later, 4 GB | A Mac with a 120 Hz screen: MacBook Pro 14 or 16 inch (2021 on, Apple M1 Pro or later), or any Apple Silicon Mac with a 120 Hz display |
| **Windows** | A 2016 laptop: Intel Core i3 or i5 6th generation (or AMD A10 / Ryzen 3 2200U), Intel HD 520 graphics or better, 4 GB, Windows 10 | Intel Core i5 10th generation or AMD Ryzen 5 3500U or later, a 120 Hz or faster screen, Windows 10 or 11 |
| **Linux** | The same as Windows, with the graphics drivers your distribution recommends (Mesa for Intel and AMD) | The same as Windows, on a 120 Hz screen |
| **Chromebook** | Intel Celeron N4000 (2019) or better, 4 GB; `Tiber.renderScale = 0.75` at full screen | A Chromebook Plus (Intel Core i3 12th generation or later) with a 120 Hz screen |
| **Raspberry Pi** | Pi 5 (any memory) at full screen; Pi 4 (2 GB or more) in a window up to 1280 by 720, or full screen with `Tiber.renderScale = 0.5`; Raspberry Pi OS with its own Chromium | Not recommended: no Pi drives 120 Hz screens well |
| **iPhone and iPad** | iPhone 8 and later, iPad (6th generation) and later, in Safari | iPhone 13 Pro and later, iPad Pro (2017 and later), ProMotion screens |
| **Android** | A 2018 phone or tablet with a Snapdragon 660 or better, in Chrome | A phone with a 120 Hz screen and a Snapdragon 865 or better |

A 2016 laptop, the oldest computer the 60 column names for Windows, has about a quarter of the single core speed of the laptop that was measured. The game's heaviest frame of work becomes about 1 ms of the 16.7 ms a frame has at 60, and drawing with its graphics chip at 1080p fits in the rest with room to spare. A Raspberry Pi 4 is about fifteen times slower on one core: the same frame of work becomes about 3 ms, and filling a full screen is what limits it, which a smaller window or `Tiber.renderScale` solves.

Two things change these numbers more than any processor does. A game's own work: a hundred thousand sparks or a long forecast every frame cost what they cost (use `swarm`, `fast` and a forecast kept from frame to frame, Section 19). And Low Power Mode on a Mac, which holds browsers to 30 frames a second whatever the game does; the developer overlay says so when that is what is happening.


### 21.6 The Switches in the Performance Monitor

The performance monitor is the developer overlay: F3 in any game, or the gauge button in the editor's run window. Under its numbers and its graph are two rows of switches, so the frame rate is changed right beside the numbers that show what it does.

**pictures** tries a game at another frame rate without touching its code:

- **GAME** is whatever the game asks for with `Tiber.frameRate` (60 when it asks for nothing).
- **60**, **120** and **240** are those frame rates, whatever the game asks for.
- **AUTO** follows the monitor: as many pictures a second as it shows, 60 on a 60 Hz screen, 120, 144 or 240 on a fast one, and back down to 60 when frames get heavy.

**updates** is GAME, 60, 120, 240 or AUTO updates a second (21.7). **GAME** is what the game asks for with `Tiber.tickRate`, and it is where every game starts. The others can only be picked for a game written to allow it, with `Tiber.tickRate = "auto"` (or `"any"`, which means the same): for any other game they are greyed out, and holding the pointer over them says why, because a game that counts its own ticks would simply run faster. For a game that allows it, GAME and **AUTO** both choose by the screen: 120 updates a second when the screen refreshes 110 times a second or more and the game's work fits, 240 when it refreshes 230 times a second or more and the work is light, 60 otherwise. The line under the switches says which it is: `this game is written for 60 updates a second`, or `this game runs 120 updates a second (AUTO: 120 on a screen of 110 Hz or more, 240 on one of 230 Hz or more, when the game's work fits)`.

The monitor is as wide as its longest line, so no number in it is ever cut short, and it is sized for the window it is in: never too small to read in the editor's run window, and larger on a big screen.

The running game changes at once, the monitor's second line says what is in charge, and both choices are remembered on that computer for every run after, until they are switched back. The monitor is in exported games too, so a player can choose there as well, the way a game's graphics settings let them. A TiberScript 1.2 game shown at 120 draws a new picture in between two updates, from where everything is on its way to the next one.

### 21.7 120 and 240 Updates a Second

`Tiber.frameRate = 120` draws twice as often, and the game still updates 60 times a second: smooth to look at, and nothing about the game changes. A game can also really run 120 updates a second, so it reads the controller, moves and checks its collisions twice as often, which a fast action game feels:

```tiberscript
init() = function
  Tiber.tickRate = "auto"
  hero = register("hero", 0, 0)
end

fixedUpdate() = function
  hero.x += 2 * Tiber.tickScale
  if hero.x > 150 then hero.x = -150 end
end

draw() = function
  screen.clear()
  screen.fillCircle(hero.x, hero.y, 6, "#78DC8C")
end
```

`Tiber.tickRate` is the updates a second: 60 (the default), 120, 240, or `"auto"`, which says the game is written to run at any of them. A game set to `"auto"` runs 120 updates a second on its own when the screen refreshes 110 times a second or more and a frame's update and drawing take under 3.5 ms on average, and goes back to 60 when the screen is slower or the work grows past 6.5 ms. From 120 it goes on to 240 when the screen refreshes 230 times a second or more and the work is under 1.6 ms, and back to 120 when the screen is under 200 Hz or the work grows past 3.2 ms. It decides over 90 frames at a time, changes only after two such checks agree, and never more than once a second, so it does not flicker between them. The player can still pick 60, 120 or 240 in the performance monitor. `"any"` is another name for `"auto"`. Reading `Tiber.tickRate` gives the rate the game runs right now, 60, 120 or 240. `Tiber.tickScale` is how much of a sixtieth of a second one update stands for: 1 at 60 updates a second, 0.5 at 120, 0.25 at 240. `Tiber.dt` is the seconds one update stands for: 1/60, 1/120 or 1/240.

What a game asks the engine for is measured in time, not in updates, and the engine does the counting:

- `seconds(n)` and every number of frames given to the engine (`wait()`, `schedule()`, `every`, `tween()`, `shake()`, `flash()`, `time.keep()`, a sequence's waits) are sixtieths of a second, and last exactly as long at 120 and 240.
- `applyPhysics()` (velocity, gravity and friction), particles, the camera's follow and sprite animations all move at the same speed at 120 and 240 as at 60.
- A recording keeps the updates a second it was made at, and plays back at it.

What a game does with its own numbers is up to it: a speed added every update (`hero.x += 2`) runs twice as fast at 120, and four times as fast at 240. Multiply it by `Tiber.tickScale`, or write it per second and multiply by `Tiber.dt` (`hero.x += 120 * Tiber.dt`), and it is the same at both. A counter of updates (`timer += 1` until 60) becomes `timer += Tiber.dt` until 1. That is all a game needs to set `Tiber.tickRate = "auto"`. `time.now` counts updates, so at 120 it counts twice as fast.

Two notes. An online game with rollback must run the same updates a second for every player: keep it at 60, or pick one rate for everyone. And a game that does not set `Tiber.tickRate` is never changed by the switch: it stays at 60 updates, and draws at whatever the pictures switch says.

A whole game can be written for both. A platform game, a climbing game and a game for a phone that set `Tiber.tickRate = "auto"` and `Tiber.frameRate = "auto"` run 120 real updates and 120 pictures a second on a 120 Hz screen, at exactly the speed they run at 60. What it takes, in a game of six thousand lines: every speed added each update multiplied by `Tiber.tickScale`, every number that eases toward another (`x += (target - x) * 0.2`) eased by `1 - (1 - 0.2) ^ Tiber.tickScale` instead, every counter of updates counted in sixtieths, and a check that something happens on one exact update (`if timer == 30`) made a check that the counter just passed it. Measured in a desktop export on a screen made to refresh 60 and then 120 times a second: 60 updates and 60 pictures at 60 Hz, 120 updates and 120 pictures at 120 Hz. And played at both rates side by side by its tests: a second of running covers the same ground, a held jump is as high and lasts as long, a bolt flies as far, a turret fires as often, and a spiral throws the same number of bullets.

### 21.8 Locked to the Screen

A screen shows a new picture at a steady beat: 60, 120 or 144 times a second. A game that updates whenever enough time has passed, by the computer's clock, sometimes updates twice in one beat and not at all in the next, and the eye sees that as a stutter, even at a high frame rate. Every TiberScript game is **locked to the screen**: when the screen's beat is a whole number of times the game's updates a second (60 updates on a 60, 120 or 240 Hz screen, 120 updates on a 120 or 240 Hz screen), the engine updates exactly once every one, two, three or four beats, never twice and never skipped, and the pictures drawn in between are shown at exactly the right part of the way. A screen slower than the game by a whole number gets the same care: at 30 pictures a second (a browser that holds the page to 30 to save the battery) the game updates exactly twice in every picture, and at 60 pictures and 120 updates twice as well, never once in one picture and three times in the next. Only when the screen's beat does not match (60 updates on a 144 Hz screen, or a frame that took too long) does it go back to counting time, by the browser's most exact clock. The game never runs faster or slower than it should: a second is still a second.

### 21.9 A Long Game Stays Fast

A game is not played for a minute. It is left open for an hour, and its last minute has to cost what its first one did. Three things the engine does keep it so, and one thing a game should do.

- **What is gone leaves.** The engine keeps a list of the objects that carry a tag, for `collide()`, `near()` and the Timeline. An object that calls `vanish()` leaves that list within half a second. The list holds what is in the game, not everything that ever was.
- **A game that never asks pays nothing.** The list is only kept for a game that uses it: one that asks about tags (`collide()`, `near()`, `touching`, `array.ofObjects()`), keeps time, has physics bodies or saves the whole game. A game that marks its bullets and never asks about a tag keeps no list at all, and its bullets are simply let go.
- **Pictures in between stay cheap.** At 120 pictures a second (21.2) the engine shows the world part of the way between two updates. It keeps what it needs for that from one update to the next instead of looking at the whole world again, and a world of more than 6,000 objects with numbers to show is drawn once an update until it has fewer, which the console says once.
- **Call `vanish()` on what is done.** A bullet off the screen, an enemy defeated, a coin picked up: an object given a tag with `mark()` stays in the world until its `vanish()` is called. A game that forgets is told. At 20,000 objects that were marked and never taken out, the console says what is happening and what to call, long before a player would feel it.

A game that makes thousands of short lived things every second is what a `swarm` is for (Part Two of Section 19): its members are numbers in columns, and one that is despawned is gone at once.


---

### 21.10 A Game for an Old Laptop

Everything in this section comes to a short list. A TiberScript game that follows it runs on a ten year old laptop and on a new one, and looks its best on each without asking the player anything.

```tiberscript
init() = function
  Tiber.frameRate = "auto"
  Tiber.renderScale = "auto"
  if Tiber.prefersLessMotion then
    Tiber.motion = 0.3
  end
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

- **Let the engine choose.** `Tiber.frameRate = "auto"` draws as often as the screen and the computer can, and `Tiber.renderScale = "auto"` draws fewer pixels when the frame rate asks for it and all of them when it does not (21.2 and 21.3). These two lines do more for an old computer than anything a game's own code can.
- **Spend where it shows.** What costs is drawing, not logic: a frame of a whole platform game's logic is a few hundredths of a millisecond (Section 19, Part Three). A thousand shapes cost more than a thousand `if`s. Draw a crowd as a `swarm` (Part Two), which hands the screen one shape for all of it, and draw what never changes once, into a layer (Section 8).
- **Scale the decoration, never the game.** The example above asks for forty sparks, and for ten when the computer is slow: `system.fps` is the frames drawn in the last second. Particles, the number of lights that throw shadows (Section 23.12), a filter on the whole frame: these can shrink and the game is the same game. Enemies, bullets and physics cannot, or the game plays differently on a slow computer.
- **Keep lights few and filters short.** A light with shadows is the heaviest thing in Section 23, and `effects.filter()` runs for every pixel of the screen. `quality = 0.5` on a shader works out a quarter of the pixels (19.37).
- **Do not make garbage.** A list made new every frame is thrown away every frame, and sooner or later the browser stops the game to clean up. Make lists in `init()`, empty them and fill them again, and let a `swarm` hold what comes and goes by the thousand.
- **Look at the numbers on that computer.** F3 shows the frame rate, the time of an update and of a draw, and the garbage collections, on the computer the game is running on. `Tiber.forge.hot()` (19.25) names the functions that take the time.
- **Respect the player's settings.** `Tiber.motion` for shake and flash (Section 13), the volume channels (Section 8), and a pause that really pauses.

A controller is part of this too. On the computer microStudio runs on, the three console makers' controllers are read the moment they report (Section 7), so an old laptop with a slow browser does not make a controller slow.

### 21.11 The Settings Screen Every Game Has

How many pictures a second, how many pixels, how hard a button has to be pressed: the right answer is different on every computer and in every pair of hands, and only the player knows theirs. So every TiberScript game has a settings screen, drawn by the engine, the same in each of them. A game does nothing to have it.

**F4 opens it**, in any game, at any moment. On a controller, START and SELECT held together for three quarters of a second do the same. The game waits behind it, exactly where it was: nothing updates while the screen is open, and the game goes on from the same tick when it closes. The arrows, the D pad or a stick move through it, and a held direction keeps moving.

| Line | What it sets | Choices |
| --- | --- | --- |
| PICTURES A SECOND | `Tiber.frameRate` | auto, 60, 120 |
| PICTURE | `Tiber.resolution` | native, 720, 1080, 1440, 4k |
| DETAIL | `Tiber.renderScale` | auto, 100%, 75%, 50% |
| SHADERS | `Tiber.shaderScale` | auto, 100%, 75%, 50%, 25% |
| SHAKE AND FLASH | `Tiber.motion` | 100%, 75%, 50%, 25%, 0% |
| RUMBLE | how strong every `rumble()` is | full, half, off |
| CONTROLLER FEEL | `controllers.feel` | fast, normal, precise, relaxed |
| BUTTON PRESS | `controllers.threshold` | auto, instant (0.05), light (0.1), normal (0.2), firm (0.4), hard (0.6) |
| STICK REST | `controllers.deadzone` | auto, none (0), tiny (0.05), small (0.1), normal (0.15), wide (0.25), widest (0.35) |
| STICK FEEL | `controllers.curve` | quick (0.7), straight (1), fine (1.5), finer (2) |
| MOMENTS | `capture.auto` and `capture.clips` (Section 26) | off, pictures, pictures and clips |
| SET UP A CONTROLLER | the screen of `controllers.setup()` | |
| AS THE GAME STARTED | puts every line back to what the game itself set | |

**What the player chose is kept.** It is kept for that game, with the game's saves, and set again each time the game starts, after `init()` has run: so what a game sets in `init()` is where the screen starts, and what a player changed stays changed. A line the player never touched is left to the game.

**A game's own menu reaches the same lines.** A game with an options menu does not have to draw all of this again. One line of its menu opens the screen, and any line it does want to draw itself reads and changes the same setting, kept the same way:

```tiberscript
init() = function
  Tiber.frameRate = "auto"
  line = 0
end

optionLines() = function
  return ["PICTURES A SECOND: " + Tiber.frameRate, "RUMBLE: " + Tiber.settings.name("rumble"), "EVERY SETTING", "BACK"]
end

update() = function
  if key("arrowDown").repeat or controllers.repeat("down") then line = (line + 1) % 4 end
  if key("arrowUp").repeat or controllers.repeat("up") then line = (line + 3) % 4 end
  if key("enter").hit or controllers.hit("confirm") then
    if line == 0 then Tiber.settings.next("frameRate") end
    if line == 1 then Tiber.settings.next("rumble") end
    if line == 2 then Tiber.settings.open() end
  end
end

draw() = function
  screen.clear("#101020")
  local lines = optionLines()
  for i = 0 to lines.length - 1 do
    local color = "#C8C8DC"
    if i == line then color = "#FFD750" end
    screen.text(lines[i], 0, 30 - i * 16, 9, color)
  end
end
```

| What | What it does |
| --- | --- |
| `Tiber.settings.open()`, `.close()` | opens and closes the screen |
| `Tiber.settings.isOpen` | `true` while it is open |
| `Tiber.settings.next(name)` | moves one line to its next choice, keeps it, and gives the new value back |
| `Tiber.settings.set(name, value)` | sets one line and keeps it |
| `Tiber.settings.get(name)` | the value of one line now |
| `Tiber.settings.name(name)` | the choice of one line as the screen writes it: `"firm"`, `"half"`, `"75%"` |
| `Tiber.settings.changed` | the names of the lines the player has changed |
| `Tiber.settings.reset()` | every line as the game started, and nothing kept |
| `Tiber.settings.rumble` | how strong rumble is, from 0 to 1 |
| `Tiber.settings.key` | the key that opens the screen, `"f4"` unless it is set. `null` leaves the opening to the game |
| `Tiber.settings.combo` | whether START with SELECT opens it, `true` unless it is set |

The names are `"frameRate"`, `"resolution"`, `"renderScale"`, `"shaderScale"`, `"motion"`, `"rumble"`, `"threshold"`, `"deadzone"` and `"curve"`.

## 22. Built In Platform SDKs: Steam, Epic, and the Consoles

A finished game wants achievements, stats, leaderboards and saves that follow the player to another computer. Every store has its own SDK for them, in its own language, and none of them is a web page's: Steamworks is C++, Epic Online Services is C, and each console's is under its own agreement. microStudio puts one small set of calls in front of all of them, called `platform`:

```tiberscript
init() = function
  platform.define("first_blood", "First Blood", "Defeat your first enemy")
  platform.define("untouched", "Untouchable", "Clear an arena without being hit")
end

enemyDefeated() = function
  platform.unlock("first_blood")
  platform.addStat("enemies", 1)
end

arenaCleared(took, hits) = function
  if hits == 0 then platform.unlock("untouched") end
  platform.submitScore("fastest_arena", 100000 - round(took * 1000))
end
```

The same code works everywhere a game runs. Where it goes is decided by where the game is played, not by the game:

| Where the game runs | `platform.name` | Where it all goes |
| --- | --- | --- |
| A browser, the editor, a web export | `"web"` | kept on the player's computer, in the game's own storage, with a small banner at the top of the screen when an achievement unlocks |
| A desktop export (Windows, Linux, macOS) with a Steam App ID, started from Steam | `"steam"` | Steamworks: Steam's achievements and their popup, stats, leaderboards, Steam Cloud, the player's Steam name, rich presence and the Steam overlay |
| A desktop export with Epic Online Services set up | `"epic"` | Epic Online Services, through the bridge a game ships for it (22.9) |
| Anything a game plugs in itself | that backend's own name | a console SDK, a web portal's API, anything (22.10, 22.11) |

A game is finished and tested in a browser first, and every achievement, stat and leaderboard works there; the same build then goes to Steam with an App ID typed in.

### 22.1 Achievements

`platform.define(id, name, description)` gives an achievement its name and description, for the banner and the list. It is optional: on Steam, the names come from the Steamworks site. `platform.unlock(id)` unlocks it, and gives back 1 the first time and 0 after, so a game can show its own celebration once. `platform.unlocked(id)` says whether it is unlocked, `platform.achievementList()` is every achievement the game defined or unlocked, each with its `id`, `name`, `description` and `unlocked`, and `platform.clearAchievement(id)` locks one again, for testing. On Steam, an achievement's id is its **API name** on the Steamworks site, letter for letter.

`platform.showToasts(false)` turns the web's banner off, for a game that draws its own. Steam shows its own popup, so the web's banner never shows there.

### 22.2 Stats

`platform.setStat(name, value)`, `platform.addStat(name, amount)` (1 when left out) and `platform.stat(name)` (0 for a stat never set) keep numbers about the player: enemies defeated, distance run, deaths. On Steam, each stat is created on the Steamworks site with the same API name; a whole number is set as Steam's INT and any other as FLOAT, and Steam is told to store them at once.

### 22.3 Leaderboards

`platform.submitScore(board, score)` sends a score to a leaderboard, and `platform.scores(board)` is its top scores, best first, each with a `rank`, a `name` and a `score`. On the web a leaderboard is this computer's ten best. On Steam, the board is found by name (and made, sorted with the highest first, if it does not exist yet), the player's best is kept, and `platform.scores()` gives this computer's list until Steam answers, then Steam's, asked again every thirty seconds while the game keeps reading it. `platform.refreshScores(board)` asks again now. Every call returns at once, so a game never waits for a store to answer: it reads what is known now, and the rest arrives on a later frame.

### 22.4 Cloud Saves

`platform.cloudSave(name, value)` keeps any value (a number, a string, a list, a whole `data` block) in the player's cloud, `platform.cloudLoad(name)` gives it back (0 when there is nothing), and `platform.cloudDelete(name)` removes it. On the web the cloud is the computer's own storage. On Steam it is Steam Cloud, which follows the player to every computer they play on: turn it on for the game on the Steamworks site (Steam Cloud, with a byte quota and a number of files).

### 22.5 The Player, Presence and the Overlay

`platform.user()` is the player, with an `id` and a `name`: on Steam, their Steam ID and their Steam name; on the web, `"local"` and `"Player"` until the game calls `platform.setPlayerName()`. `platform.setPresence(text)` says what the player is doing, for their friends to see ("Fighting the final boss"). `platform.openOverlay(what)` opens the store's overlay, where there is one: `"achievements"`, `"friends"`, `"stats"`, `"community"`, `"players"`, `"settings"`, `"store"`, or a web address. It gives back 1 when it opened and 0 where there is none (the web). `platform.ready` is 1 once the platform has answered, and `platform.events()` lists the last twenty achievements and scores the game sent.

### 22.6 Outside TiberScript

`platform` is not only TiberScript's: it is one of microStudio's own globals, the same calls in every language microStudio runs, each spelled that language's own way. TiberScript 1.1, frozen exactly as it shipped, does not have `platform`; every 1.1 program runs on 1.2, which does.

### 22.7 Setting Up Steam

1. Make the game on the Steamworks site, which gives it an **App ID**, and add its achievements, stats and leaderboards there, with the same API names the game uses.
2. In the editor, open the project's **Settings**, show the advanced options, and type the App ID into **Platform SDKs**. While testing, 480 is Valve's own test game, Spacewar, which every Steam account can use.
3. The desktop export carries Steam's library through **steamworks.js**, which is open source and free. It goes into microStudio once, in `server/build/local/sdk/steamworks.js` (the package, unpacked: its own folder, with its `package.json` and its `dist` folder inside). Every desktop export of a game with a Steam App ID then includes it, and one without an App ID never does.
4. Export the game for Windows, Linux or macOS from the Publish tab, and start it with Steam running. The export has a `steam_appid.txt` beside it, so it finds its App ID when started outside Steam while you test; a build uploaded to Steam is started by Steam itself.

microStudio now carries steamworks.js 0.4.0 in that folder (8.9 MB, MIT licensed, with Steam's library for Windows, Linux and macOS inside), so a desktop export of a game with an App ID has it with nothing to set up. That version of steamworks.js has achievements, cloud saves, the player's name, the overlay, and stats as whole numbers (`platform.setStat()` rounds a number to the nearest whole one and sends it; a stat made in Steamworks as a float would need a newer steamworks.js). It has no leaderboards: until a version that does is put in the same folder, `platform.submitScore()` and `platform.scores()` in a Steam export keep this computer's list, as on the web, and say nothing is wrong. Put a newer steamworks.js in the folder and Steam's leaderboards are used at once, with no change to the game.

If steamworks.js is not there, or Steam is not running, the exported game says so in its log and keeps everything on the computer, as on the web: it never stops working. Tested as far as a computer without a Steam account for the game allows: the calls, the bridge between the game and the app around it, and the export carrying all of it are tested; Steam's own answers can only be seen with a real App ID, on Steam.

### 22.8 What Each Store Needs

| | Achievements | Stats | Leaderboards | Cloud saves | Player and presence | Overlay |
| --- | --- | --- | --- | --- | --- | --- |
| **Web** | on the computer, with a banner | on the computer | the computer's ten best | on the computer | `"Player"`, or the name the game sets | none |
| **Steam** | Steamworks achievements, Steam's popup | Steamworks stats | Steam leaderboards | Steam Cloud | Steam name and ID, rich presence | the Steam overlay |
| **Epic** | what the bridge sends to EOS Achievements | EOS Stats | EOS Leaderboards | EOS Player Data Storage | the Epic account, EOS Presence | the Epic overlay |

### 22.9 Setting Up Epic Online Services

Type the game's Product ID, Sandbox ID, Deployment ID and Client ID, from the Epic Developer Portal, into **Platform SDKs**. Epic Online Services is a C library with no JavaScript version of its own, so a game that ships on the Epic Games Store adds a small bridge for it: a file, `server/build/local/sdk/eos-bridge.js`, that the desktop export carries. It has one function, `start(config)`, that is given the ids and gives back an object with the same calls the Steam one answers: `info()` (the `name` `"epic"`, and the `user`), `unlock(id)`, `setStat(name, value)`, `submitScore(board, score)`, `scores(board)`, `cloudSave(name, text)`, `cloudLoad(name)`, `cloudDelete(name)`, `setPresence(text)` and `openOverlay(what)`. Inside, it calls the EOS SDK, which the studio downloads from the Epic Developer Portal under Epic's terms, through a native addon built for it. microStudio cannot ship the EOS SDK itself, and says so plainly: the bridge is the one piece a studio writes, once, and every game's code stays the same.

### 22.10 PlayStation, Nintendo and Xbox

Sony (PlayStation Partners), Nintendo (the Nintendo Developer Portal) and Microsoft (the GDK, through ID@Xbox) give their SDKs only to developers they have accepted, under agreements that keep the SDKs and everything about them private. No engine can put them in a download for everyone, and none of these consoles runs a game the way a computer's browser does: a game reaches a console through a port the studio makes with the console's own tools. What microStudio gives is the part that does not change: a game written with `platform` keeps every line of its achievements, stats, leaderboards and saves when it gets there, and the studio, once accepted, plugs the console's SDK in behind `platform.use()`.

### 22.11 A Backend of Your Own

```javascript
platform.use({
  name: "myportal",
  unlock: id => fetch("/api/trophy/" + id, { method: "POST" }),
  submitScore: (board, score) => fetch("/api/score", { method: "POST", body: JSON.stringify({ board, score }) }),
  scores: (board, done) => fetch("/api/scores/" + board).then(r => r.json()).then(done),
  user: () => ({ id: window.portalUser.id, name: window.portalUser.name })
})
```

`platform.use(backend)`, from JavaScript (or from TiberScript's `javascript` block), plugs in anything with some of these: `name`, `unlock(id)`, `clearAchievement(id)`, `setStat(name, value)`, `submitScore(board, score)`, `scores(board, done)` (calling `done` with the list when it arrives), `cloudSave(name, value)`, `cloudLoad(name)`, `cloudDelete(name)`, `user()`, `setPresence(text)`, `openOverlay(what)`, and `ownToasts: true` for a backend that shows its own popup. What it leaves out is kept on the computer, as on the web, so a backend can start with one call and grow. A web game portal, a school's own leaderboard, a console port, and a test that checks what a game sends are all one object.

---

## 23. Light and Effects

A dark cave lit by one torch, a hero's lantern that throws the shadows of the pillars across the floor, a flashlight beam in a haunted house, the long shadows of a forest at sunset, a thousand embers that each light the smoke around them: every 2D game wants these, and almost no 2D game on the web has them, because lighting has always meant a second drawing pass, render targets and a week of work. In TiberScript it is one line a light.

Light has a section of its own, beside the biggest update and not inside it, because the biggest update is for what changes what a game can be, and a light, as good as it is, is a better way of drawing. It stands on Part Four of that update, the Graphics Chip (19.27): both are drawn by the browser's graphics chip.

### 23.1 The Dark, and a Light

```tiberscript
init() = function
  lighting.ambient = 0.15
  hero = register("hero", 0, 0)
end

update() = function
  hero.x = mouse.x
  hero.y = mouse.y
end

draw() = function
  screen.clear("#556070")
  screen.fillRect(0, -60, 300, 20, "#8A7A62")
  screen.light(hero.x, hero.y, 90, "#FFD080")
end
```

`lighting.ambient` is how bright the dark is, from 0 (black) to 1. It starts at 1, which is no dark at all, so nothing changes in a game until it asks. Below 1, at the end of `draw()` the engine builds a **light map**: the dark, with every light of the frame added onto it, each brightest at its center and fading softly to nothing at its edge. Then the whole frame is multiplied by it. Where a white light is full, things look exactly as they were drawn; where there is no light, they are as dark as `lighting.ambient`; and a colored light tints what it falls on, an orange torch making grey stone warm.

`screen.light(x, y, radius, color, strength)` is one light for this frame, in the world's coordinates like everything else (the camera and a shake move it with the world). `color` is any color TiberScript draws with and `strength` is 1 when left out: 0.5 is half as bright, and 2 is twice as bright, which washes things out toward white like a light that is too close. Because the frame is multiplied, draw everything in its full, lit colors and let the dark take them down: a wall drawn nearly black stays nearly black even in full light.

`lighting.tint` is the color of the dark: `lighting.tint = "#6070C0"` makes it moonlight blue instead of grey. `lighting.glow`, from 0 to 1 (0.35 when left out), is light scattered in the air around each light: a soft colored haze that shows even on dark things, and a bright heart at the light itself. It never shows inside a shadow. `lighting.quality`, from 0.125 to 1 (0.5 when left out), is how many of the screen's pixels the light map has: light is soft anyway, so half is enough, and 0.25 is cheaper still on a slow computer. `lighting.off()` puts every setting back as it started.

Nothing about a light is kept from one frame to the next: a light that is not drawn this frame is gone. That is what lets it rewind, replay and roll back with the rest of the game for nothing (23.12).

### 23.2 Shadows

```tiberscript
init() = function
  lighting.ambient = 0.1
  crates = [vector(-60, 0), vector(40, 30)]
end

draw() = function
  screen.clear("#6A6470")
  for c in crates do
    screen.fillRect(c.x, c.y, 20, 20, "#B08850")
    lighting.block(c.x, c.y, 20, 20)
  end
  screen.light(mouse.x, mouse.y, 160, "#FFFFFF")
end
```

`lighting.block()` puts something in the light's way for this frame, and every light it stands in front of throws its shadow behind it, as far as the light reaches. It takes:

- a box: `lighting.block(x, y, width, height)`, centered on `x` and `y` like `screen.fillRect()`,
- an object: `lighting.block(enemy)`, a box the size of its `w` and `h` (16 when it has none),
- a tilemap: `lighting.block(level)`, every solid tile (`.solid()`, or every tile that is not empty when none were named), with the tiles side by side in a row made into one box, and only the part of the map near the screen,
- a swarm: `lighting.block(rocks)`, a box for every member,
- or a list of any of these.

`lighting.blockLine(x1, y1, x2, y2)` is a thin wall from one point to another, for a slope, a fence or the edge of a cliff.

The thing itself is not left black. `lighting.walls`, from 0 to 1 (0.5 when left out), is how much of the light a blocker's own face takes, so a pillar in a torch's light shows as a lit pillar with its shadow behind it. A light that stands inside a blocker, a torch on a wall, is not shut in by that blocker.

`lighting.shadows = false` turns every shadow off, and `screen.light(x, y, r, color, 1, shadows = false)` turns one light's off. A light with shadows costs more than one without, about the same as drawing it twice more, so a game with hundreds of small lights keeps shadows for the few big ones.

### 23.3 Round Things, and Soft Shadows

```tiberscript
init() = function
  lighting.ambient = 0.12
  lighting.softness = 0.5
  rocks = [vector(-50, 10), vector(30, -30), vector(70, 40)]
end

draw() = function
  screen.clear("#5A6070")
  for r in rocks do
    screen.fillCircle(r.x, r.y, 14, "#9A8C80")
    lighting.blockCircle(r.x, r.y, 14)
  end
  screen.light(mouse.x, mouse.y, 180, "#FFE8C0")
end
```

`lighting.blockCircle(x, y, radius)` puts a round thing in the light's way: a rock, a barrel, a planet, a ball. Its shadow is as wide as the circle seen from the light, and it moves around it as the light does, which a box cannot do. It counts for `lighting.at()` the same way a box does.

`lighting.softness`, from 0 to 1 (0.25 when left out), is how soft the edges of shadows are. At 0 they are cut sharp, like light from a single bulb; at 0.5 they fade over a little distance, like light from a lamp with a shade; at 1 they are as soft as light through a cloudy window. Softer shadows also hide the corners a box makes. On a browser that cannot blur, shadows are sharp whatever this says.

### 23.4 Spotlights and Flicker

```tiberscript
init() = function
  lighting.ambient = 0.08
  aim = 0
end

update() = function
  aim = atan2(mouse.y, mouse.x)
end

draw() = function
  screen.clear("#505868")
  screen.spotlight(0, 0, 220, aim, PI / 4, "#E6F0FF")
  screen.light(-120, 60, 70, "#FF9040", 1, flicker = 0.4)
end
```

`screen.spotlight(x, y, radius, angle, width, color, strength)` is a cone of light: `angle` is where it points and `width` is how wide the whole cone is, both in radians like `atan2()`, `cos()` and `sin()` (0 points right, `PI / 2` up). A flashlight, a car's headlights, a lighthouse, a guard's view. Shadows fall inside the cone the same way.

`flicker = ` from 0 to 1 makes any light waver like a flame: 0.2 is a candle in still air, 0.5 a torch in the wind. Each light flickers in its own rhythm, worked out from where it is, so two torches never flicker together.

Any light can also be given its values by name: `screen.light(x = 10, y = 20, radius = 60, color = "#FFFFFF", strength = 0.8, shadows = false, flicker = 0.3)`.

### 23.5 Sunlight

```tiberscript
init() = function
  lighting.ambient = 0.25
  lighting.tint = "#4050A0"
  trees = [vector(-90, -20), vector(-20, 10), vector(60, -10)]
  sunset = 0
end

update() = function
  sunset += 0.002
end

draw() = function
  screen.clear("#6A8050")
  for t in trees do
    screen.fillCircle(t.x, t.y, 12, "#2E5A30")
    lighting.blockCircle(t.x, t.y, 12)
  end
  screen.sunlight(-PI / 2 + sin(sunset) * 0.9, 0.9, "#FFD8A0", 140)
end
```

`screen.sunlight(angle, strength, color, length)` is light from very far away: it falls on the whole world at once, all of it traveling the same way, so every shadow points the same way and they are all as long as each other, the way shadows are outside on a sunny day. `angle` is the way the light travels, in radians like `atan2()` (`-PI / 2`, the default, is straight down the screen, a sun high in the sky behind the player; nearer 0 or `PI` it comes in from the side, low in the sky, and the shadows grow long). `strength` is 0.8 when left out, `color` a warm white, and `length` (160 when left out) is how far a shadow reaches behind the thing that throws it.

Move the angle over a few minutes and the shadows swing across the ground as the day goes on; turn the strength down and the tint to blue and it is evening. Sunlight is added to the dark like any other light, so a torch still glows warm in a shadow, and `lighting.at()` knows where the shade is: a vampire that must keep out of the sun asks it.

### 23.6 A Light on Every Spark

```tiberscript
spark = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.vy = randomFloat(1, 3)
  end
end

sparks = swarm of spark max 5000

init() = function
  lighting.ambient = 0.2
end

fixedUpdate() = function
  if sparks.count < 300 then sparks.spawn(randomFloat(-150, 150), -90) end
  sparks.each do (s)
    s.y += s.vy
    if s.y > 100 then s.despawn() end
  end
end

draw() = function
  screen.clear("#404858")
  sparks.draw(circle = 1.5, color = "#FFD080")
  sparks.lights(10, "#FFB050", 0.5)
end
```

`swarm.lights(radius, color, strength)` puts a light on every member of a swarm, read straight from the swarm's columns of numbers without making a single object. Embers, fireflies, lasers, magic missiles, a city's windows at night. A swarm's lights cast no shadows unless asked (`sparks.lights(10, "#FFB050", 0.5, shadows = true)`), because a crowd of small lights is about color and not about shadow.

A frame takes up to 4096 lights. Past that the rest are left out, and the console says so once. `lighting.count` is how many lights this frame has so far, and `lighting.lastCount` how many the last frame had.

### 23.7 A Glow That Lights Nothing

```tiberscript
init() = function
  lighting.ambient = 0.1
end

draw() = function
  screen.clear("#30343C")
  screen.fillRect(0, 30, 120, 8, "#FF40A0")
  screen.glow(0, 30, 70, "#FF40A0", 0.8)
  screen.fillCircle(-40, -20, 3, "#FF3020")
  screen.fillCircle(-30, -20, 3, "#FF3020")
  screen.glow(-35, -20, 14, "#FF3020", 1)
end
```

`screen.glow(x, y, radius, color, strength)` is a soft halo of light that shows even in the dark and lights nothing around it: a neon sign, a monster's eyes in a cave, a pool of lava seen from far away, a button the player should press, an item to pick up. It is drawn after the dark, so the dark never covers it, and it is not a light: it throws no shadows, `lighting.at()` does not count it, and a hundred of them cost about what a hundred circles do. `strength` is 1 when left out.

A light makes a place bright; a glow makes a thing look bright. Most glowing things want both: a lamp is a `screen.light()` for the room and a small `screen.glow()` for the bulb.

### 23.8 How Bright Is It Here?

```tiberscript
init() = function
  lighting.ambient = 0.1
  guard = register("guard", 80, 0)
  thief = register("thief", -80, 0)
end

fixedUpdate() = function
  if not lighting.dark(thief.x, thief.y) and distance(thief.x, thief.y, guard.x, guard.y) < 120 then
    print("spotted!")
  end
end

draw() = function
  screen.clear("#505868")
  screen.light(guard.x, guard.y, 100, "#FFE0A0")
end
```

`lighting.at(x, y)` is how bright a place is, from 0 to 1: the dark, plus every light that reaches it and is not blocked on the way, by how bright that light's color looks to an eye. It is the same light the player sees, so a stealth game hides the thief in exactly the shadows on the screen. `lighting.dark(x, y)` is true when a place is darker than 0.35, and `lighting.dark(x, y, 0.2)` picks the line.

It reads the lights of the last frame drawn (or of this frame, once a light has been added to it), with the settings as they are now. It needs no page: it answers the same in a test, on a server and in a browser. Light is drawn, not kept, so inside a forecast or a rollback it reads the last picture actually shown: for a decision that must come out the same on every player's machine in an online game, work it out from your own positions (how far the thief is from the lamp) instead.

`lighting.colorAt(x, y)` is the color of the light at a place, as `"#RRGGBB"`: the dark's tint, plus every light and sunlight that reaches it, each in its own color. A chameleon that takes the color of the light it stands in, a plant that only grows under the blue lamp, or a crystal that sings when red light falls on it: `lighting.colorAt(hero.x, hero.y)` tells it. Like `lighting.at()`, it needs no page.

### 23.9 A HUD That Stays Lit

```tiberscript
init() = function
  lighting.ambient = 0.15
  score = 0
end

draw() = function
  screen.clear("#505868")
  screen.light(0, 0, 80, "#FFFFFF")
  lighting.draw()
  screen.text("SCORE " + score, 0, 80, 10, "#FFD750")
end
```

Left to itself, the dark goes on at the end of `draw()`, over everything the game drew. `lighting.draw()` puts it on there and then, once a frame: what is drawn after it, a score, a health bar, a map, is drawn at full brightness. With `screen.layer()`, the dark goes on in the layer `lighting.draw()` was called in, so the layers above it stay lit.

Particles from `emit()`, the dialogue box, touch buttons, the controller setup screen and the debug overlay are drawn after the dark and are never darkened: a spark from `emit()` glows on its own.

### 23.10 Effects: a Finish on the Whole Frame

```tiberscript
init() = function
  effects.crt = 0.8
  effects.grain = 0.3
end

draw() = function
  screen.clear("#102030")
  screen.text("GAME OVER", 0, 0, 20, "#FF5050")
end
```

`effects` are put on the whole finished frame, the very last thing before it is shown: the HUD, the dialogue and the particles included, the way a television shows everything the console sends it. Each is a number from 0 (off, where they all start) to 1:

| Effect | What it does |
| --- | --- |
| `effects.bloom` | bright things glow and bleed light into what is around them |
| `effects.scanlines` | thin dark lines across the screen, like an old monitor |
| `effects.vignette` | the corners fall into shadow, drawing the eye to the middle |
| `effects.chromatic` | the red and the blue drift apart at the edges of things, like a cheap lens |
| `effects.grain` | film grain that moves every frame |
| `effects.crt` | an old television: scanlines, a vignette, color fringes and bloom at once; each of those set on its own as well uses whichever is stronger |
| `effects.pixelate` | big pixels: their size in the game's units (2 or more; 0 is off), for a retro look or a transition |
| `effects.tint` | a color laid over everything (`"#FF0000"` for a hit, `"#203060"` for night), as strong as `effects.tintAmount` (0.25) |

`effects.active` lists the ones that are on, and `effects.off()` turns them all off. They can change every frame: `effects.pixelate` going from 16 to 0 is a scene transition, and `effects.tint = "#FF0000"` for a few frames is a hit.

Light and effects are on the page itself, so a game in a browser, in the editor and in every export shows them the same. Without a page (a test, the server), nothing is painted, and everything else, `lighting.at()` included, is exactly the same.

### 23.11 Filters: Your Own Shaders, Stacked on the Frame

```tiberscript
shader heatWave(u, v, t) = function
  local bend = sin(v * 50 + t * 6) * 0.003
  return pixel(u + bend, v)
end

shader coolEdge(u, v) = function
  local d = distance(u, v, 0.5, 0.5)
  return color(0.1, 0.2, 0.5, smoothstep(0.3, 0.8, d) * 0.6)
end

init() = function
  heat = effects.filter(heatWave, 0)
  cool = effects.filter(coolEdge)
  cool.strength = 0.7
  clock = 0
end

update() = function
  clock += 1 / 60
  heat.set(clock)
  if key("h").hit then heat.on = not heat.on end
end

draw() = function
  screen.clear("#203040")
  screen.fillCircle(mouse.x, mouse.y, 20, "#FFD050")
  screen.text("HOT", 0, 60, 20, "#FFFFFF")
end
```

`effects.filter(shader, ...)` puts one of your own shaders (Section 19, Part Four) on the finished frame, every frame, until it is taken off: a filter, like the ones a photo editor stacks. It returns the filter, and the filter has:

- `.strength`, from 0 to 1 (1 when left out): how much of the filter is mixed over what is under it. 0.5 is half.
- `.on`: false skips it without taking it off the stack.
- `.set(...)`: new values for the shader's own parameters, the ones after `u` and `v`. Above, the heat wave's time, every update.
- `.remove()`: takes it off the stack.

Filters run in the order they were added, each one reading the frame the one before it made through `pixel()`. A color the shader returns with an alpha below 1 is laid over the frame, as `coolEdge` above darkens only the corners. `effects.filters` is the stack as a list, and `effects.clearFilters()` empties it.

They are the frame's own effects, the same as `effects.crt` and the rest (23.10): the HUD, the dialogue and the particles are all inside them, and `effects.crt` and the others go on after the last filter. On the graphics chip the frame goes up once, through every filter one after the other without coming back, and down once: a stack of four filters costs a little more than one, not four times as much. Without a graphics chip each filter runs on the processor, as a shader always can.

### 23.12 What It Costs

The light map is drawn by the browser's graphics chip, and what the game's own code spends on it is small. In a scene of seven lights, four of them throwing the shadows of seven blockers, the light costs a median of 0.1 ms of a frame's script time on a recent laptop (the drawing itself runs on the graphics chip alongside), and a frame with 4096 swarm lights costs under 50 ms to prepare even with no graphics chip at all. Sunlight costs about what one light with shadows does. Soft shadows are one blur over the whole light map, once a frame, not one for every light. And the light map never has more pixels than a 1280 by 720 frame times `lighting.quality` squared, however big or sharp the screen: light is smooth, so a sharp screen loses nothing to it, and in a browser where the light was the slowest part of the frame, a room of seven lights on a sharp screen went from 28 to over 100 pictures a second. The debug overlay (`Tiber.showDebugOverlay(true)`) has a line for it: how many lights, how many with shadows, how many blockers, and what the light and the effects cost this frame.

Light is drawn, every frame, from where things are, and nothing about it is kept. That is why it works with Section 19 with no work at all:

- **Time** keeps the world, and the light follows: rewind the hero and their lantern is back where they were, with no light state to save, rewind or send over a network. A replay, a forecast drawn as a ghost, `save.game()` and a rollback all light correctly without a line of code for it.
- **Swarms** are columns of numbers, and `swarm.lights()` reads them straight, so a crowd can carry its own light.
- `lighting.at()` and `lighting.colorAt()` are pure arithmetic over the same lights, so the rules of a stealth game, a plant that grows toward the light or an enemy that fears it are as exact as the picture.


### 23.13 Mirrors and Water

Two things are called a mirror, and they go together. One throws light back. The other shows the picture turned over.

**A mirror that throws light back.** `lighting.mirror(x1, y1, x2, y2)` is a mirror from one point to another. No light passes it, like a wall. And every light that reaches it shines again from behind it, as far behind as the light is in front, through the mirror and nowhere else: what a real mirror does. The light thrown back is a light like any other, so it has shadows, a spotlight comes back as a beam pointing the other way, and `lighting.at()` counts it.

```tiberscript
init() = function
  lighting.ambient = 0.1
  lamp = register("lamp", -60, 0)
end

draw() = function
  screen.clear("#20242E")
  screen.light(lamp.x, lamp.y, 160, "#FFD9A0")
  lighting.mirror(40, -70, 40, 70)
  lighting.block(-10, 40, 16, 16)
  screen.fillRect(40, 0, 3, 140, "#9FD0FF")
  lighting.draw()
  bright = lighting.at(0, 0)
end
```

The room in front of the mirror is lit twice, once by the lamp and once by what the mirror throws back, the crate throws a second shadow away from the mirror, and behind the mirror it is dark. A fifth value is how much the mirror throws back, from 0 to 1 (0.85 unless said): `lighting.mirror(40, -70, 40, 70, 0.5)` is a dull one. `lighting.bounced` is how many lights were thrown back in the frame drawn last. A light is thrown back once: two mirrors facing each other do not go on for ever.

**A mirror that shows the picture.** `screen.mirror(x, y, w, h)` is a rectangle of the screen that shows, turned over, the picture next to one of its edges. Which edge is its `side`: `"down"` (what is below it, the mirror on a wall that shows the floor in front of it, and what `screen.mirror()` does unless told), `"up"`, `"left"` or `"right"`. It is painted from what has been drawn so far this frame, so it shows sprites, shapes, text and shaders alike, and what is drawn after it is in front of it.

`screen.water(x, y, w, h)` is the same thing with the look of water: it shows what is above it, it ripples, it is tinted blue, and it fades with depth. A lake, a puddle or a polished floor is that one line, called after the world is drawn and before whatever stands in front of the water.

```tiberscript
init() = function
  hero = register("hero", 0, 20)
  t = 0
end

update() = function
  t += 1
  hero.x = sin(t / 40) * 80
end

draw() = function
  screen.clear("#1B2238")
  screen.fillRect(0, 60, 400, 80, "#2A3350")
  screen.fillRect(hero.x, hero.y, 14, 22, "#FFD750")
  screen.water(0, -50, 400, 100)
  screen.mirror(120, 60, 50, 40, "down", 0.8, tint = "#C0E0FF")
end
```

| Value | What it does | Mirror | Water |
| --- | --- | --- | --- |
| `side` | the edge whose picture is shown: `"up"`, `"down"`, `"left"`, `"right"` | `"down"` | `"up"` |
| `strength` | how strongly it shows, from 0 to 1 | 0.9 | 0.55 |
| `tint`, `tintAmount` | a color laid over the reflection, and how much of it | none | blue, 0.3 |
| `fade` | how much fainter it is at its far edge, from 0 to 1 | 0 | 0.75 |
| `ripple` | how far the picture is pushed about, in the screen's units; 0 is still | 0 | 1.5 |
| `speed` | how fast the ripples move | 1 | 1 |
| `scale` | below 1 squeezes more of the picture in, the way shallow water does | 1 | 1 |
| `blur` | softens it | 0 | 0 |

The side and the strength can follow the size as plain values, and every one of them can be given by name: `screen.water(0, -50, 400, 100, ripple = 3, tint = "#103050")`.

They belong with everything else here. A reflection is painted before the light, so the lights and the dark fall on it as on the rest of the picture, and a lantern over a lake is reflected lit. Effects and filters (23.10, 23.11) are a finish on the whole frame, the reflection included. Nothing about a mirror or water is kept from one frame to the next: rewind the game and the reflection is of the world as it was, and the ripples move with the game's time, so they stop when time stops and slow when it slows. `Tiber.hasFeature("mirrors")` and `Tiber.hasFeature("water")` say the engine has them.

What it is not: a mirror shows what is on the screen beside it, so something off the screen is not in it, and a mirror does not show what another mirror shows.

---

## 24. Physics That Rewinds

Crates that fall and stack, a ball that bounces lower every time and rolls, a wall that stops what slides into it: real physics, with one line a body. And the thing no physics engine has done before: it is part of the world the Timeline keeps (Section 19, Part One). A rewind puts every crate back exactly, a forecast shows a thrown ball's real bounces before it is thrown, a replay and an online rollback play them again the same, and every computer and phone gets the same numbers to the last digit.

### 24.1 Bodies

```tiberscript
crate = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    physics.add(this, "box", 16, 16)
  end
end

init() = function
  floor = register("floor", 0, -80)
  physics.add(floor, "box", 400, 20, fixed = true)
  crates = []
  for i = 0 to 5 do
    crates.push(new crate(0, -62 + i * 17))
  end
  ball = register("ball", -60, 60)
  physics.add(ball, "circle", 8, bounce = 0.8)
end

draw() = function
  screen.clear("#203040")
  screen.fillRect(floor.x, floor.y, 400, 20, "#556070")
  for c in crates do
    screen.fillRect(c.x, c.y, 16, 16, "#C08850")
  end
  screen.fillCircle(ball.x, ball.y, 8, "#78DC8C")
end
```

`physics.add(object, "box", width, height)` and `physics.add(object, "circle", radius)` make an object a body. From then on, once every tick, it falls with `physics.gravity` (-0.35 when left out: the speed it gains each sixtieth of a second, downward), hits the other bodies and stops, and is pushed by what hits it. Its `x` and `y` are its center and `vx` and `vy` its speed, the same fields everything else in TiberScript uses, so the game draws it, moves it and reads it the way it always has. Boxes stay upright; a circle that rolls turns, and its `angle` (in degrees) says how far.

Each body takes named values after its size:

- `fixed = true`: it never moves. A floor, a wall, a platform.
- `bounce =` from 0 to 1 (0.2): how much of its speed it keeps when it hits something. A slow hit (slower than six ticks of falling) does not bounce, it lands.
- `friction =` (0.4): how much it holds on as it slides. 0 is ice.
- `mass =`: how heavy it is, for what it pushes and what pushes it. When left out, its size decides.
- `gravity =` (1): how much of `physics.gravity` it feels. 0 floats, -1 rises.

`physics.add(level)` with a tilemap (`level = tilemap("level1")`, Section 11) makes its solid tiles something every body stands on and bumps into.

### 24.2 Pushing, and What Touches What

```tiberscript
init() = function
  floor = register("floor", 0, -80)
  physics.add(floor, "box", 400, 20, fixed = true)
  ball = register("ball", 0, -60)
  physics.add(ball, "circle", 8)
end

fixedUpdate() = function
  if key("space").hit and ball.grounded then physics.push(ball, 0, 6) end
  if key("right").held then physics.push(ball, 0.1, 0) end
  if physics.touching(ball, floor) then
    ball.onFloorTicks = (ball.onFloorTicks or 0) + 1
  end
end
```

`physics.push(body, x, y)` is a shove: it changes the body's speed by that much, divided by its mass, so a heavy crate moves less for the same push. Setting `vx` and `vy` directly works too.

After every step each body knows what it touches: `body.contacts` is the list of bodies (and tilemaps) touching it, `body.grounded` is true when it stands on something, and `physics.touching(a, b)` asks about two. A jump that only works from the ground, a crate that breaks when something lands on it, a pressure plate: each is one line.

`physics.remove(body)` makes it an ordinary object again, `physics.paused = true` holds every body still, `physics.iterations` (8) is how carefully contacts are settled each step (more for tall stacks), `physics.count` is how many bodies the last step moved, and `physics.bodies()` lists them.

### 24.3 It Rewinds, Forecasts and Rolls Back Exactly

```tiberscript
init() = function
  time.keep(seconds(10))
  floor = register("floor", 0, -80)
  physics.add(floor, "box", 400, 20, fixed = true)
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
  screen.fillRect(floor.x, floor.y, 400, 20, "#556070")
  forecast 90 each (future) do
    screen.fillCircle(future.ball.x, future.ball.y, 1, "#FFD750")
  end
  screen.fillCircle(ball.x, ball.y, 6, "#78DC8C")
end
```

Hold R and the ball flies back along its bounces; let go and it bounces again exactly the same. The dots are where it will be, every tick of the next second and a half, bounces included, and the ball then goes through every dot.

This works because the physics keeps nothing of its own. Everything a body is lives on the object: where it is, its speed, its shape and its material, what it touches. The bodies are found, every step, in the world's own list of objects. So when the Timeline puts the world back (a rewind, a forecast, a replay, a `whatif`, a rollback, `save.loadGame()`), the physics is back with it, with nothing to save, restore or send. And the step itself is deterministic: the bodies are always met in the same order, and it uses only `+`, `-`, `*`, `/` and the square root, which every computer and phone rounds the same way. Checked by the tests: a rewind of 90 ticks and the 90 ticks played again match to the last digit, and a forecast of 60 ticks matches the 60 ticks that then happen.

The physics steps right after `fixedUpdate()` (or `update()` in a game without one), in a real tick, inside a forecast and inside a rollback alike, and not in a tick that rewinds. At 120 updates a second (Section 21.7) a step is half as long, and at 240 a quarter: a body in the air follows the exact curve gravity gives it, so it falls as far in a second at 120 and at 240 as at 60.

### 24.4 What It Costs, and What It Does Not Do

Measured on a recent laptop: 300 crates in ten rows take 0.2 ms a tick, and settle. The debug overlay has a line for it: how many bodies, how many contacts, and the time.

Boxes stay upright and do not tip over, and there are no joints, ropes or springs; for a spinning platform or a chain, move the bodies yourself each tick and the physics treats them as moving. Swarm members are not bodies (a crowd of thousands wants `each touching`, Section 19, Part Two).

---

## 25. Swarms the Graphics Chip Moves

```tiberscript
shader drift(x, y, vx, vy, t) = function
  local n = noise(x * 0.01, y * 0.01, t)
  return color(x + vx + n, y + vy, vx * 0.99, vy - 0.02)
end

dust = archetype
  init(x, y) = function
    this.x = x
    this.y = y
    this.vx = randomFloat(-1, 1)
    this.vy = randomFloat(0, 2)
  end
end

motes = swarm of dust max 200000

init() = function
  for i = 1 to 5000 do
    motes.spawn(randomFloat(-150, 150), randomFloat(-80, 80))
  end
  clock = 0
end

fixedUpdate() = function
  clock += 1 / 60
  motes.moveOnChip(drift, clock)
end

draw() = function
  screen.clear("#02030A")
  motes.drawPoints(1.5, "#C8D8FF")
end
```

`swarm.moveOnChip(shader, ...)` hands every member of a swarm to the graphics chip, which runs the shader once for each of them, all at once, and writes what it returns back into them. The shader's first parameters are the swarm's own fields, by name (`x`, `y`, `vx`, `vy` above, up to four), and what it returns is their new values in the same order: a `color(...)` carries four numbers, `r` into the first field, `g` into the second and so on. A shader that returns a number changes only the first field. The parameters after the fields are yours, given after the shader (`clock` above).

It is the same `shader` as Section 19, Part Four, with the same rules (it cannot read `pixel()` here: a member is not a picture), and where there is no graphics chip the processor runs it on every member instead, with the same result.

When it pays: when each member's math is heavy. Measured on a recent laptop, a million members, each moved through six layers of `noise()`: 19.7 ms a pass on the graphics chip, 146 ms on the processor, 7 times faster. For a simple step (add the speed, wrap at the edge) `each fast` (Section 19, Part Three) is faster still, because handing a million members to the chip and back costs more than the arithmetic: 200,000 stars move in 1.5 ms with `each fast` and 7.5 ms on the chip. The chip works in 32 bit numbers where the processor works in 64, so a member moved on the chip can differ from the processor's answer in the seventh digit: right for anything the player looks at, a reason to keep what decides a game (and what a rollback compares) with `each fast`.

---

## 26. Pictures and Clips of the Best Moments: capture

A player remembers a few moments of a game: the boss going down, a jump made by a hair, a room seen for the first time. Nobody presses a key to keep them, because by the time a key could be pressed the moment is gone. `capture` keeps them for the player: as pictures, and as short clips with the seconds that led up to them.

```tiberscript
init() = function
  bind("dash", "shift")
  capture.auto = "auto"
  capture.onAction("dash")
  kept = 0
  on("captured") do (item)
    kept += 1
    print("kept " + item.name + " at tick " + item.tick)
  end
end

bossDefeated() = function
  capture.moment("the boss falls")
end
```

`capture.shot(name)` keeps a picture of this frame, as it looks when the frame is finished, HUD and all. `capture.moment(name)` does the same, and when clips are on it keeps a clip as well: about four seconds before the moment and two after it. The same name asked for twice in one frame is kept once.

**The engine finds moments by itself.** `capture.auto` is `false` when a game starts: nothing is kept, and the engine does not even tell capture what happens, so a game that never asks pays nothing. Set to `true`, the engine takes its own pictures of three things:

- **A sprite seen for the first time**, once the game has been running for two seconds: the rare enemy, the secret room's treasure, even a sprite that is on the screen for a single frame. A sprite that was never there is not a picture, and after one of these the next waits two seconds.
- **A sudden burst of action**: many particles at once, a shake, a flash, many sounds together, far more than the game's own usual. A big moment for a quiet puzzle game is a small one in a shooter, so each game is measured against itself. After one burst the next waits three seconds.
- **The actions a game names**: `capture.onAction("dash")` keeps a picture each time that action is pressed, at most twice a second. `capture.offAction(name)` stops it.

`capture.auto = "auto"` turns all of this on only on a computer with the memory for it (eight gigabytes or more), and leaves it off on a small one. `capture.on` says whether it is on now, on this computer.

**Clips.** `capture.clips = true` keeps the last few seconds of the game, small, a picture every fourth frame, in a ring that is written over as the game goes on, so a moment comes with what led to it. The ring is sized by the computer's memory: a computer with four gigabytes or less keeps half as much, smaller.

**What is kept, and how to use it.** `capture.list` is every picture and clip kept, oldest first; each has a `name`, a `kind` (`"picture"` or `"clip"`), the `tick` it was taken on, and its `width` and `height`. `capture.last` is the newest and `capture.count` how many there are. The sixteen newest pictures and the four newest clips are kept (eight and two on a computer with four gigabytes or less), and each picture is turned into a small compressed file as soon as it is taken. `capture.save(item)` hands one to the player as a file, a picture as a PNG and a clip as a video; `capture.clear()` lets every one of them go. The `"captured"` event (Section 6) runs for each one kept, with the picture or clip.

When something is kept, a notice in the corner says so, for two seconds: `PICTURE: the boss falls`. `capture.quiet = true` keeps the notice away, for a game that shows its pictures its own way.

**What is never a moment.** A forecast, a replay or a tick the Timeline is working out (Section 19) is not the real present, and nothing in it is kept.

**The player decides too.** The settings screen every game has (21.11) has a line for it, MOMENTS: off, pictures, or pictures and clips. What the player chooses there is kept for that game, like every other line, and comes after what the game itself set.

```tiberscript
init() = function
  capture.auto = true
  capture.quiet = true
  shots = 0
end

update() = function
  if key("p").hit then
    capture.shot("by hand")
  end
  shots = capture.count
end

draw() = function
  screen.clear("#101020")
  screen.text("pictures kept: " + shots, 0, 0, 10, "#FFFFFF")
end
```

A game is free to show what it kept: a gallery in its menu, the best moment of a run on its game over screen, a picture next to a high score. Capture never stops a game to do its work: a picture is copied at the end of a frame, and turned into a file while the game goes on.

---

## The Full Example

A small example script cannot honestly show a full length game. This one uses the module system itself to spread across a few real files, the same way an actual project would, and between them exercises nearly everything documented above alongside everything carried forward from 1.1, the biggest update included: the Timeline, a swarm of boss shots moved with `fast`, the Forge under all of it, JavaScript and a buffer beside the TiberScript, a lit arena (Section 23), and every controller.

```tiberscript
// enemies.tbs
export damageable = trait
  takeDamage(amount) = function
    this.hp -= amount
    if this.hp <= 0 then this.die() end
  end
end

export slime = archetype uses damageable
  export moveSpeed = 1
  export contactDamage = 10

  init(x, y) = function
    this.x = x
    this.y = y
    this.hp = 30
    this.mass = 1
    this.mark("enemy")
    this.collisionShape("circle", 12)
    this.collisionLayer("enemy")
  end

  die() = function
    trigger("enemyDied", data(points = 100, x = this.x, y = this.y))
    emit("boom", this.x, this.y, 8)
    this.vanish()
  end

  draw() = function
    screen.fillCircle(this.x, this.y, 12, "green")
  end
end

export goblin = archetype extends slime
  init(x, y) = function
    super.init(x, y)
    this.hp = 60
    this.contactDamage = 20
  end

  chase(targetX, targetY) = function
    local toTarget = vector(targetX - this.x, targetY - this.y)
    if toTarget.length() > 4 then
      local dir = toTarget.normalize()
      this.x += dir.x * this.moveSpeed
      this.y += dir.y * this.moveSpeed
    end
  end

  draw() = function
    screen.fillCircle(this.x, this.y, 14, "red")
  end
end
```

```tiberscript
// main.tbs
import "enemies"

states = enum
  menu
  game
  paused
  gameOver
end

hero = archetype uses enemies.damageable
  signal died()
  signal healthChanged(oldHp, newHp)
  export moveSpeed = 3

  init(name) = function
    this.name = name
    this.x = 0
    this.y = 0
    this.hp = 100
    this.mass = 2
    this.friction = 0.1
    this.mark("player")
    this.collisionShape("circle", 14)
    this.animate("walk", ["hero_walk1", "hero_walk2", "hero_walk3", "hero_walk4"], 8)
    this.animate("idle", ["hero_idle1", "hero_idle2"], 20)
  end

  takeDamage(amount) = function
    local old = this.hp
    this.hp -= amount
    this.healthChanged.trigger(old, this.hp)
    if this.hp <= 0 then this.died.trigger() end
  end

  die() = function
    this.died.trigger()
  end
end

config = data
  difficulty = "normal"
  maxLives = 3
end

shot = archetype
  init(x, y, vx, vy) = function
    this.x = x
    this.y = y
    this.vx = vx
    this.vy = vy
    this.life = seconds(3)
    this.mark("bossShot")
  end
end

shots = swarm of shot max 2000

timeless rewindsUsed = 0

landOnFloor(o) = function
  if o.y < -80 then
    o.y = -80
    o.vy = 0
  end
end

byDistance(a, b) = function
  return distance(player.x, player.y, a.x, a.y) < distance(player.x, player.y, b.x, b.y)
end

heal(amount) = function
  local old = player.hp
  player.hp = min(player.hp + amount, 100)
  player.healthChanged.trigger(old, player.hp)
  sfx("heal")
end

onPlayerDied() = function
  transit(states.gameOver)
end

onHealthChanged(oldHp, newHp) = function
  if newHp < oldHp then shake(4, 10) end
end

on("enemyDied") do (info)
  score += info.points
  sfx("kill")
end

init() = function
  bind("moveRight", "arrowRight")
  bind("moveRight", "d")
  bind("moveLeft", "arrowLeft")
  bind("moveLeft", "a")
  bind("jump", "space")
  bind("jump", controllers.button("south"))
  bind("heal", "h")
  bind("heal", controllers.button("north"))
  bind("rewind", "r")

  seedRandom(1234)
  time.keep(seconds(10))
  score = 0
  coinCount = 0
  caption = ""
  minions = []

  player = new hero("Adventurer")
  player.died.connect(onPlayerDied)
  player.healthChanged.connect(onHealthChanged)

  boss = new enemies.goblin(120, 0)

  repeat(5) do
    local coin = register("coin" + coinCount, random(-150, 150), random(-80, 80))
    coin.mark("currency")
    coinCount += 1
  end

  gravity(0.4)
  camera.follow("player", 0.1)
  camera.bounds(-500, -300, 500, 300)
  audio.setVolume("music", 0.6)

  best = save.get("highScore", 0)
  transit(states.menu)
end

entering states.game do
  tune("dungeon_theme")
  sequence do
    caption = "A boss awaits..."
    wait(seconds(1))
    caption = ""
  end
end

entering states.gameOver do
  silence("music")
  if score > best then
    save.set("highScore", score)
    save.write()
  end
end

fixedUpdate() = function
  during states.game do
    if action("rewind").held and time.kept > 0 then
      time.rewind(2)
      rewindsUsed += 1
    else
      if action("moveRight").held then player.x += player.moveSpeed end
      if action("moveLeft").held then player.x -= player.moveSpeed end
      if action("jump").hit then player.vy = 8 end
      if action("heal").hit then heal(25) end
      applyPhysics("player")
      applyPhysics("boss")
      landOnFloor(player)
      landOnFloor(boss)
      boss.chase(player.x, player.y)
      for m in minions do
        m.chase(player.x, player.y)
      end

      attempt
        local hit = collide("currency")
        if hit then
          score += 100
          sfx("ching")
          hit.vanish()
        end
      recover
        debug("coin collision check failed")
      end

      local nearBoss = near("enemy", 16)
      if nearBoss then
        resolve(player, nearBoss)
        player.takeDamage(1)
      end

      every(seconds(2)) do
        minions.push(new enemies.goblin(randomFloat(-150, 150), randomFloat(-80, 80)))
      end

      every(seconds(1)) do
        local a = angle(boss.x, boss.y, player.x, player.y)
        for spread in [-0.3, 0, 0.3] do
          shots.spawn(boss.x, boss.y, cos(a + spread) * 2, sin(a + spread) * 2)
        end
      end
      shots.each fast do (s)
        s.x += s.vx
        s.y += s.vy
        s.life -= 1
        if s.life <= 0 then s.despawn() end
      end
      shots.each touching "player" do (s, p)
        p.takeDamage(5)
        s.despawn()
      end
    end
  end
end

update() = function
  during states.menu do
    lighting.ambient = 1
    screen.clear("black")
    screen.text("Press SPACE", 0, 0, 20, "white")
    if key("space").hit then transit(states.game) end
  end

  during states.game do
    lighting.ambient = 0.35
    screen.clear("#222")

    if action("moveRight").held or action("moveLeft").held then
      player.playAnimation("walk")
    else
      player.playAnimation("idle")
    end

    screen.layer(0)
    screen.fillRect(0, 0, 400, 220, "#111")

    screen.layer(1)
    player.draw()
    boss.draw()
    for m in minions do
      m.draw()
    end
    shots.draw(circle = 2, color = "#FF5050")
    shots.lights(8, "#FF5050", 0.4)
    screen.light(player.x, player.y, 120, "#FFE6B0")
    lighting.draw()

    // the ghost of where the boss will be in half a second
    local ahead = time.forecast(30)
    alter opacity=0.3 do
      screen.fillCircle(ahead.boss.x, ahead.boss.y, 14, "red")
    end

    screen.layer(2)
    ui.healthbar(player.hp, 100, -100, 90, 200, 16)
    screen.text("Score: $(score)  Best: $(best)", -150, -95, 15, "white")
    screen.text(caption, 0, 20, 20, "white")
    if time.rewinding then screen.text("<< REWIND", 0, 60, 16, "#5AA0FF") end
  end

  during states.gameOver do
    lighting.ambient = 1
    screen.clear("black")
    screen.text("Game Over", 0, 10, 40, "red")
    screen.text("Score: $(score)", 0, -20, 18, "white")
    if key("space").hit then transit(states.menu) end
  end
end
```

```tiberscript
// enemies_test.tbs
import "enemies"

test "slime dies at zero hp" do
  local s = new enemies.slime(0, 0)
  s.takeDamage(30)
  assert(s.hp <= 0)
end

test "goblin has more hp than slime" do
  local g = new enemies.goblin(0, 0)
  local s = new enemies.slime(0, 0)
  assert(g.hp > s.hp)
end
```

And a fourth file shows the controls the way the player's own controller prints them, rumbles when the hero is hurt, and lets a player set up a controller the browser does not know:

```tiberscript
// controls.tbs
import "enemies"

controlsHint() = function
  local pad = controllers.first
  if pad == null then return controllers.hint end
  return "jump " + pad.label("south") + "   heal " + pad.label("north")
end

feelHit() = function
  for pad in controllers.list do
    pad.rumble(0.5, 0.15)
  end
end

drawControls(x, y) = function
  local pad = controllers.first
  if pad == null then return 0 end
  pad.drawButton("south", x, y, 10)
  pad.drawButton("north", x + 40, y, 10)
  if not pad.setUp then
    screen.text("press C to set up your " + pad.name, x, y - 14, 6, "#FFB050")
    if key("c").hit then controllers.setup(pad.player) end
  end
end
```

A fifth file is the part of the game that reaches past TiberScript: a block of JavaScript for what only the page knows, a buffer of heat that a `fast` function spreads as WebAssembly, an old helper kept by the 1.1 rules it was written for, and a loading step that picks how finely the game is drawn and leaves nothing for the Forge to compile later. The game calls `workshop.warmUp()` once in `init()`, `workshop.spread(workshop.heat)` each tick, and `workshop.shareScore(score)` when it is over:

```tiberscript
// workshop.tbs
export javascript
  function today() {
    return new Date().toLocaleDateString()
  }
  function shareScore(points) {
    if (navigator.share) navigator.share({ text: "I scored " + points + "!" })
  }
end

export heat = buffer(64)

export fast spread(h) = function
  for i = 0 to h.length - 1 do
    h[i] = (h[i - 1] + h[i] * 2 + h[i + 1]) * 0.245
  end
  return h.length
end

tiberscript 1.1
  export lastStep(n) = function
    for i = 1 to n do
    end
    return i
  end
end

export warmUp() = function
  Tiber.resolution = "1080p"
  Tiber.shaderScale = "auto"
  return Tiber.forge.compileAll()
end
```

For a multiplayer variant of the same game, one line in `init()` is all it takes, because every input already goes through actions and every change already happens in `fixedUpdate()`:

```tiberscript
init() = function
  network.rollback(players = 2, delay = 2)
end
```

And for a game that sends its own messages instead, the hooks from Section 16 still work:

```tiberscript
network.on("playerJoined") do (remote)
  register("player_" + remote.id, 0, 0)
end

network.on("position") do (msg, fromPlayer)
  local remote = table.get(otherPlayers, fromPlayer.id, null)
  if remote then
    remote.x = msg.x
    remote.y = msg.y
  end
end
```

A separate addon file can customize the shared `enemies.tbs` without ever editing it, which matters once that file is something other people are also using. Inside an extension, `super.die()` calls the method it replaced, so the addon adds its blood and the slime still announces its death and vanishes:

```tiberscript
// gory_addon.tbs
import "enemies"

Tiber.extendArchetype(enemies.slime, "die", function()
  emit("blood", this.x, this.y, 20)
  super.die()
end)
```
