# The Mégra Editor

Mégra comes with it's own editor, which is a special-purpose editor for an s-expression based language and comes with 
plenty practical commands that make editing easier. 

Some of the shortcuts are inspired by Emacs.

Given that it's build for one specific purpose, it's not configurable at all. 
If you **don't** want to use the editor, you can start Mégra with the `-r` flag, which will start it in REPL mode. Feel free to make 
your own plugin for your favourite editor, and let me know it you do so!

## The Sketchbook

On the top of the editor, you can find a dropdown menu with your Mégra sketches, which you can find in your sketchbook folder (see the Folders section).

## Commands

* `Ctrl+Return` executes the current s-expression.
* `Ctrl+Space` toggles selection mode.
* `Backspace` deletes the previous character, `Ctrl+Backspace` deletes the previous word.
* The Editor uses electric parenthesis. You can enclose a section of the code in parenthesis by selecting it an typing a single `(`.
* Copy: `Ctrl+C` Paste: `Ctrl+V` or `Ctrl+Y`
* `Tab` formats the current s-expression.
* Arrow keys move the cursor. `Ctrl+<ArrowKey>` moves the cursor by words, not characters.
* `Ctrl+F` moves the cursor forward.
* `Home` moves the cursor to the first open parenthesis of the current line.
* `End` moves the cursor to the end of the current line.
