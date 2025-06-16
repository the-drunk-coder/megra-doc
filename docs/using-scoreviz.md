# Using Scoreviz

Scoreviz is a dynamic score generator to interact with instrumental musicians. It's still
very much work in progress, but can be used to create scores with text, images, and small
note snippets on-the-fly.

It's currently not integrated in Mégra, but a browser-based application written in JavaScript.

It is controlled via OpenSoundControl (OSC) and could potentially be used from any OSC-Capable
program, but it's mostly designed to work with Mégra.

## Installation

Follow the instructions in the README here: [https://github.com/the-drunk-coder/scoreviz](https://github.com/the-drunk-coder/scoreviz)

## Run Scoreviz

Start scoreviz from a terminal by navigating to the folder where you stored it and running `node scoreviz.js`.

Open a browser and go to `http://localhost:8082`.

## Scoreviz

In the Mégra repository, you'll find a folder called `scoreviz_interface` that contains two files, 
`scoreviz.megra3` and `scoreviz_tutorial.megra3`.

[https://github.com/the-drunk-coder/megra.rs/tree/main/scoreviz_interface](https://github.com/the-drunk-coder/megra.rs/tree/main/scoreviz_interface)

Place both files in your [sketchbook folder](https://megra-doc.readthedocs.io/en/latest/folders/#sketchbook-folder).

Now, start Mégra and have a look at the `scoreviz_tutorial.megra3` file. This will guide you through the process of creating dynamic scores.

The `scoreviz.megra3` file contains convenience functions to work with Scoreviz. There's no formal documentation as of now but each
function is commented.
