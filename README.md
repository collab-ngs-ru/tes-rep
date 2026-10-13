# tes-rep

## Cistern — a thief in the sewers

`sewer.html` is a single self-contained file: all CSS and JS inline, no external
requests, no build step. Open it in a browser and click.

A first-person sewer crawl in the manner of *Thief: The Dark Project* — lamplight
pooled in black brick corridors, a wheellock pistol, and little angry robot
terrapins that sprint, bark and bite. Take the four reliquaries, leave by the
east grate.

* **W A S D** walk · **Shift** run · **Space** jump · **Ctrl / C** slide (while running)
* **Mouse** look, left button shoots (hold to keep firing) · **R** reload · **Esc** releases the mouse

Two shots scrap a terrapin, one through the head; two bites finish you.
The light gem at the bottom of the screen tells you how visible you are —
lamplight gets you seen, running gets you heard.

### How it works

* Software raycaster drawing into an `ImageData` buffer: DDA walls, cast floor
  and ceiling, depth-tested billboards, scaled up for a coarse, grainy picture.
* Wall, floor, water and sprite art is drawn procedurally at load, never loaded.
* Lighting is a lightmap baked once from nine guttering lamps with line-of-sight,
  sampled bilinearly, plus a muzzle flash that briefly lights the whole corridor.
  Enemy sight checks read the same bake, so shadow is real cover.
* Every sound — pistol, bark, bite, boots, drips, room tone — is synthesised with
  the Web Audio API and panned by where the thing is standing.
