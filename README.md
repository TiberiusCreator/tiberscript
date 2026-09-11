# TiberScript

**TiberScript** is a custom game engine and scripting language built inside microStudio to streamline game development, featuring a unique syntax focused on 4-way movement, state management, and clean visuals. While version 1.0 introduced the custom interpreter and direct-boot gameplay, the fully remastered **TiberScript 1.1** runs on Python and brings the original vision to life with a powerful real parser.

**Key Features of TiberScript 1.1**

* **Advanced Parsing:** The engine correctly handles extra spaces, blank lines, and comments, while supporting real script-level variable assignments and full mathematical or logical expressions.


* **Robust Control Flow:** Write clean logic using `if/elsetry/else` blocks, `for` and `while` loops, and easily create user-defined functions.


* **Comprehensive Inputs:** Track any keyboard key—not just arrows and space—along with precise mouse tracking (`mouse.x`, `mouse.y`, `mouse.hit`, `mouse.held`).


* **Dynamic Engine Tools:** Objects auto-create dynamically without engine edits, while scoped `alter` and `view` blocks apply real camera and opacity changes.


* **Built-in Physics:** Simplifies movement with native helpers like `gravity()`, `applyPhysics()`, `bounce()`, and `distance()`.


* **Extensibility & Debugging:** Catch issues with `attempt/recover` error handling and internal logging, modularize games using `Tiber.addScript()`, and create custom commands without touching the core engine using `Tiber.addCommand()`.
