# TiberScript

Read them in this order. Each version keeps everything the one before it had. microStudio runs TiberScript 1.2 for every project set to TiberScript. 1.1 is kept, frozen and hidden from the language menu, for projects that already use it, and 1.2 reads code written for 1.0 and 1.1 by their own rules, as a block or as a whole file.

| Document | What it is |
| --- | --- |
| TiberScript 1.0 Handbook | The first TiberScript: the core of the language, with every example running on today's engine. |
| TiberScript 1.0 Tutorial | The 1.0 course, ending with the Dungeon of Tiber and Tiberian Odyssey, a whole game in one file. |
| TiberScript 1.1 Handbook | TiberScript 1.1, exactly as its engine runs it: every keyword and built in function, the rest of microStudio reachable from TiberScript, its limits, and what a 1.0 reader should know. |
| TiberScript 1.1 Tutorial | The 1.1 course, ending with Dodge and Reach and Tiberian Odyssey 1.1, the whole game grown with what 1.1 added. |
| TiberScript 1.2 Handbook | The language microStudio runs for every project set to TiberScript, whole: the language rounded out, modules, the standard library, game feel, controllers of every brand, and Section 19, THE BIGGEST UPDATE OF ALL, in six parts: Time Is a Value (rewind, forecast, replay, rollback, and a game online in one word), A Million Things at Once (swarms), the Forge (TiberScript compiled to JavaScript, blocks of JavaScript in full inside TiberScript, `fast` compiled to WebAssembly, buffers, `wasm` and `Tiber.forge`), the Graphics Chip (shaders written in TiberScript or GLSL), HTML Powered by the Whole Forge (pages written in the game's own file, their values always up to date), and Change the Past. Then frame rate and resolution on every computer, the Settings and PerfMonitor libraries and screens of your own over the game, the platform SDKs, light and effects, and physics that rewinds. Along the way: every text of a game in another language with `Tiber.translate`, libraries of your own for every project, and the checks that find mistakes as a game loads. |
| TiberScript 1.2 Tutorial | The 1.2 course, lesson by lesson, ending with a lit Boss Arena on a shader floor that rewinds, fires a swarm moved with `fast`, and plays with a keyboard or a controller, and Tiberian Odyssey 1.2, the whole game written the 1.2 way. |

The texts for one version stand on their own: a Handbook says everything, and its Tutorial teaches the same things one lesson at a time. Nothing in them needs the internet.

Every code block in the Handbooks and Tutorials is run on the engine by microStudio's own tests, and every complete program in them is played to its end. A code block that stops working stops a test, so the texts and the engine cannot drift apart.
