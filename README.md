# LogicLab

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Live demo](https://img.shields.io/badge/demo-live-brightgreen.svg)](https://senolgulgonul.github.io/logic/)
[![No install](https://img.shields.io/badge/install-none-blue.svg)](https://senolgulgonul.github.io/logic/)

A web-based digital logic simulator for **combinational and sequential circuits**, built for teaching introductory logic design. The component symbols, flip-flops, and timing conventions follow the style used in M. Morris Mano's *Digital Design*, so it maps directly onto a typical first course.

It runs entirely in the browser as a single self-contained HTML file — no install, no build step, no server, no dependencies.

### ▶ [Try it live](https://senolgulgonul.github.io/logic/)

<!-- Add an animated demo (build → power → timing diagram) named demo.gif to the repo and it will show here -->
![LogicLab demo](demo.gif)

## Why LogicLab

Most browser and desktop logic simulators either need an install (Java, a desktop app) or draw their timing diagrams on an *event* axis, where the clock visibly stretches whenever an input changes — confusing for students reading "given the inputs, draw the output" problems. LogicLab is built around two ideas:

- **Zero friction** — one HTML file, opens in any browser, nothing to install or sign up for.
- **Honest timing diagrams** — the clock runs on a uniform timebase and stays a clean square wave; inputs appear at their true position within a cycle. The waveform reads like a logic-analyzer trace, which is exactly what an intro course needs.

## Features

- **Combinational logic** — switches, LEDs, and the seven distinctive-shape gates (NOT, AND, OR, NAND, NOR, XOR, XNOR) drawn in ANSI/Mano style with proper inversion bubbles.
- **Sequential logic** — edge-triggered D, JK, and T flip-flops as labeled boxes with the clock dynamic-indicator notch and a bubbled Q′ output. Feedback loops (Q′→D toggles, ripple counters) are supported.
- **Uniform-time timing diagram** — automatically traces every clock, input, and output as a classic stepped waveform on a real time grid, so the clock never distorts when an input changes.
- **Labeled I/O** — inputs and outputs are named on the canvas (`IN1`, `IN2`, `CLK1`, `OUT1`, `OUT2`…) matching the timing-diagram rows. **Double-click** any input or output to rename it (e.g. `A`, `B`, `Sum`, `Carry`); the name shows on the sheet and in the trace, and is saved with the circuit.
- **Manual stepping** — pause the clock and advance one rising edge at a time to walk a class through a circuit edge by edge.
- **Rotate, pan, zoom** and orthogonal wire routing with manual bend points.
- **Save / Open** circuits as plain JSON, plus a built-in **Examples** menu.
- Wires show logic level at a glance: green for 1, grey for 0.

## Getting started

No installation required.

1. Download `index.html` (or clone the repo).
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari).

```bash
git clone https://github.com/senolgulgonul/logic.git
cd logic
# then open index.html in your browser
```

The app opens to an empty sheet — use the **EXAMPLES** menu to load a ready-made circuit, or build your own from the palette.

To make it available online, enable **GitHub Pages** for the repository (it's already live at <https://senolgulgonul.github.io/logic/>) so students can use it without downloading anything.

## How to use

- **Place a part** — click it in the left palette, then click the sheet. Hold **Shift** to place several.
- **Wire** — drag from an output lead to an input lead. While wiring, click on the sheet to drop right-angle bend points. One wire per input.
- **Power** — click **POWER** to energize the circuit.
- **Toggle an input** — click an `IN` switch (when powered).
- **Clock** — the `CLK` part runs automatically; set its speed (1–10 Hz) with the slider. **❚❚** pauses it; **STEP** then advances one rising edge at a time.
- **Rename I/O** — double-click an `IN`, `CLK`, or `OUT` to give it a meaningful name.
- **Rotate** — select a component and press **R** or the rotate button.
- **Move / delete** — drag a part to move it; select a part or wire and press **Delete**. **Esc** cancels.
- **Zoom / pan** — mouse wheel to zoom; Shift-drag (or middle-mouse drag) to pan.
- **Help** — the **?** button summarizes all of the above in the app.

## Components

| Group | Parts |
|-------|-------|
| I/O | `IN` switch, `CLK` clock, `OUT` LED |
| Gates | NOT, AND, OR, NAND, NOR, XOR, XNOR |
| Flip-flops | D, JK, T (positive edge-triggered) |

## Timing diagram

The panel along the bottom records a waveform for each clock, input switch, and output. Samples are taken on a uniform time grid, so the clock stays a clean square wave while inputs appear at their true position within a cycle.

The trace deliberately focuses on the circuit's **interface** — clocks, inputs, and outputs — to keep the panel readable; internal flip-flop states show up in the outputs they drive. Use **CLEAR TRACE** to restart the trace, or **HIDE** to collapse the panel for combinational work.

> Note: flip-flops resolve to a clean 0/1 even when data changes exactly at the clock edge — metastability from setup/hold violations is **not** modeled, which is a deliberate simplification for teaching.

## Examples

Built-in (via the **EXAMPLES** menu):

- **D flip-flop** — basic edge-triggered storage element.
- **Half adder** — XOR (sum) and AND (carry) from two inputs.
- **Clock divider (÷2)** — a D flip-flop with Q′→D; output toggles at half the clock rate.
- **2-bit counter** — two toggle stages chained into a ripple counter.

## Saving and loading

**SAVE** prompts for a filename and downloads the circuit as a `.logiclab.json` file. **OPEN** loads one back. The format is plain, human-readable JSON:

```json
{
  "app": "LogicLab",
  "format": 1,
  "comps": [
    { "id": 1, "type": "SWITCH", "x": 256, "y": 112, "rot": 0, "on": false, "name": "A" }
  ],
  "wires": [
    { "id": 5, "from": { "c": 1, "p": 0 }, "to": { "c": 3, "p": 0 }, "points": [] }
  ],
  "nextId": 9
}
```

Each component stores its `type`, position, rotation, and switch value; inputs and outputs may carry an optional `name`. Each wire stores its endpoints (component id `c`, pin index `p`) and any routing bend `points`. Because the files are plain JSON, they are easy to share, diff, or hand-author.

To add a saved circuit to the **Examples** menu, paste its JSON into the `EXAMPLES` array near the bottom of the script as `{ name: "Title", circuit: <json> }`.

## Technical notes

- A single HTML file with vanilla JavaScript and inline SVG rendering — no frameworks or build tools.
- The only external resource is the IBM Plex font from Google Fonts (the app works offline without it, just with a fallback font).
- The simulation uses event-driven evaluation with fixed-point settling for combinational logic and rising-edge detection for flip-flops.

## Contributing

Issues and pull requests are welcome — especially additional teaching examples, new component types, and UI improvements. Saved circuits (`.logiclab.json`) make great example contributions.

## License

Released under the [MIT License](LICENSE).

## Acknowledgements

- Symbol and timing conventions follow M. Morris Mano, *Digital Design*.
- Inspired by desktop and web logic simulators such as [Digital](https://github.com/hneemann/Digital), Logisim, and CircuitVerse.
