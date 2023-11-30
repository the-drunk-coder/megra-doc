# The Mégra Editor

Mégra comes with it's own editor, which is a special-purpose editor for an s-expression based language and comes with 
plenty practical commands that make editing easier. 

Some of the shortcuts are inspired by Emacs.

Given that it's build for one specific purpose, it's not configurable at all. 
If you **don't** want to use the editor, you can start Mégra with the `-r` flag, which will start it in REPL mode. Feel free to make 
your own plugin for your favourite editor, and let me know if you do so!

## Fonts and Font Size

Font and font size can be specified when starting Mégra. Included fonts are *mononoki* and *Comic Mono*. 

Using the `--font` argument at startup you can specify one of the two included font names, or a path to a custom font file in ttf format.

Font size is configured using the `--font-size` argument.

Currently, font and font size can't be changed at runtime.

## The Sketchbook

On the top of the editor, you can find a dropdown menu with your Mégra sketches, which you can find in your sketchbook folder (see the Folders section).

## Commands

* `Ctrl+Return` executes the current s-expression.
* `Ctrl+Space` toggles selection mode.
* `Backspace` deletes the previous character, `Ctrl+Backspace` deletes the previous word.
* The Editor uses electric parenthesis. You can enclose a section of the code in parenthesis by selecting it an typing a single `(`.
* Copy: `Ctrl+C` Paste: `Ctrl+V` or `Ctrl+Y`
* `Tab` formats the current s-expression.
* `Ctrl+T` toggles the current s-expression on or off (by adding comments)
* Arrow keys move the cursor. `Ctrl+<ArrowKey>` moves the cursor by words, not characters.
* `Ctrl+F` moves the cursor forward.
* `Home` moves the cursor to the first open parenthesis of the current line.
* `End` moves the cursor to the end of the current line.

## Karl-Yerkes-Mode

When started with the flag `--karl-yerkes-mode`, the Mégra editor starts in a special mode inspired by Karl's **Clavm** system. Every keystroke will immediately
evaluate the current top-level expression the cursor is in. HANDLE WITH CARE! If you change a value from, say `0.2` to something else, and delete the period, you'll have 
a value of 2 ... on gain values that might be painful!
