# Unicode Piano Roll (UPR) language

## Scores

|                                                                                   |                                                                   |
| --------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| ![Unicode Piano Roll code example from the documentation](./docs/images/code.png) | ![musical score rendered from that code](./docs/images/score.png) |

...wherein:

- `D̅` is single-column shorthand for `D♯`
- `D̲` is single-column shorthand for `D♭`

## Performances

Unicode Piano Roll can also express performances of scores, where each note in the score is given a timestamp and an intensity value.

- Each timestamp is on a discrete time scale of whatever precision you choose

- Each note intensity is an integer in the range [0, 127] (for now)

In addition to those precise numbers (encoded as hex of varints of deltas), a visual overview of the note intensities is included:

![Unicode Piano roll example that includes performance intensities](./docs/images/performance.png)

This makes use of codepoints from the Block Elements block of Unicode, so:

- Showing a chord's _average intensity_ can use four glyphs per note (e.g. `███▌`) to express the intensity with 32-possible-values precision

- Showing a chord's _relative distribution_ of intensity can use one glpyh per note in the chord (with 8 possible vertical values each). For example, `▄▇` shows at a glance that the higher note in this 2-note chord is about 40% more intense.

You will also, optionally, be able to include a visualization of each note's delta between when it was predicted to be played and when it was actually played.

(predicted based on the score plus the context of the performance up to that point)

## Performing a score

### ...with evenly-distributed note intensity in the chords:

Using any of:

- `a MIDI keyboard` + `the command-line tool` (using CoreMIDI/etc)
- `a MIDI keyboard` + `the VSCode extension` (using CoreMIDI/etc)
- `a MIDI keyboard` + `a browser supporting the Web MIDI API` + `a webshit version of the command-line tool` (via compilation to WASM)

...you can press any key on the left half of your MIDI keyboard to play the next chord that's tee'd up in the score for the left hand.

This means that as a novice, without having to worry about pressing the correct keys on the keyboard you can:

- focus on learning how to play the rhythm well

- enjoy performing the piece, exploring what various timing and intensity subtleties sound like

When you trigger the playing of a chord:

1. Virtual MIDI events will immediately be sent to your system, so the notes can be played as audio from a virtual instrument in GarageBand, MainStage, or any other such app

2. If you wish, the preformance data will be logged. It can be logged to a separate performance file in the background, or you can have the program update the UPR text on the screen as you perform your way through the score, adding performance annotations in the right margin

### ...with a non-uniform distribution of note intensity in the chords:

Instead of having the entire left half of the keyboard correspond to "play the next chord for the left hand", this is a little more complicated.

You can designate a specific list of MIDI keys on the left half of the keyboard (e.g. F♯<sub>1</sub> G♯<sub>1</sub> A♯<sub>1</sub> C<sub>2</sub>) to correspond, respectively, to the first, second, third, and fourth notes of whatever chord is being played.

As soon as you press each key, the program will play the corresponding note of the chord.

When you have played each note of the chord, the program is ready to advance to the next chord for the left hand of the score.

## Recommended filenames

|                                         |                                                                                          |
| --------------------------------------- | ---------------------------------------------------------------------------------------- |
| `My_awesome_composition.upr.txt`        | when you save a score as normal plain text (UTF-8)                                       |
| `My_awesome_composition.upr.artc`       | when you save a score as an [`.artc` file](https://artcformat.org/) (with the same info) |
| `My_awesome_composition-take1.upr.txt`  | when you save a performance as normal plain text (UTF-8)                                 |
| `My_awesome_composition-take1.upr.artc` | when you save a performance as an [`.artc` file](https://artcformat.org/)                |
