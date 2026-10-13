# tes-rep

## Cistern — a thief in the sewers

`sewer.html` is a single self-contained file: all CSS and JS inline, no external
requests, no build step. Open it in a browser and click.

A first-person sewer crawl in the manner of *Thief: The Dark Project* — lamplight
pooled in black brick corridors, a wheellock pistol, and one-eyed iron vermin the
size of a small dog. Take the four reliquaries, leave by the east grate.

* **W A S D** walk · **Shift** run · **Space** jump · **Ctrl / C** slide (while running)
* **Mouse** look, left button shoots (hold to keep firing) · **R** reload · **Esc** releases the mouse

Two shots scrap one of them, or a single shot through that eye; two bites finish
you. The light gem at the bottom of the screen tells you how visible you are.

### Listening

Sound is the other half of the game, so it is worth wearing headphones.

* Everything is positioned in 3D and panned through an HRTF panner with the
  listener locked to your head and facing, so left, right, behind and above are
  all distinguishable. A wall between you and a sound takes its gain and most of
  its top end away, and sends more of it to the reverb instead — a scrabbling
  you can hear *through* brick sounds nothing like one in your corridor.
* Your own boots are driven by the same noise value the vermin hear on. A crouch
  is a scuff that barely leaves your feet; a walk is a boot; a run slaps the
  brick and rings the whole cistern. **If you can hear the room answer your
  footsteps, so can they** — that ringing is roughly the radius you are audible in.
* The room itself is a convolution reverb built at load from a generated
  stone-cistern impulse, under a high shelf and a compressor, so it sits warm
  rather than tinny.

### The vermin

Armoured quadrupeds with a single lamp of an eye, built out of about thirty
boxes each: haunches, hunched spine plates, four legs on hip pivots that walk in
diagonal pairs, a segmented tail, ears, whisker rods and an iron jaw. They prowl,
hear your boots, investigate, yap, and mostly just *trot* — slower than your
walk. Every twenty seconds or so one will wind up with a screech and spend a
one-and-a-half second charge at better than six metres a second. The eye flares
white-hot while it dashes, which is your warning, and the gap between dashes is
the window to break line of sight or put a ball through it.

### How it works

* Software raycaster drawing into an `ImageData` buffer: DDA walls, cast floor
  and ceiling, then a small flat-shaded box rasteriser for everything solid —
  vermin, reliquaries and lamps are real geometry, depth tested per pixel against
  the same inverse-depth buffer the walls and floor write into. Only light with
  no surface (eye bloom, lamp flame) is still billboarded.
* Wall, floor and water textures are drawn procedurally at load, never fetched.
* Lighting is a lightmap baked once from nine guttering lamps with line-of-sight,
  sampled bilinearly, plus a muzzle flash that briefly lights the whole corridor.
  Enemy sight checks read the same bake, so shadow is real cover.
* Render resolution adapts: if frames start costing more than ~21 ms it drops the
  buffer width and climbs back when there's headroom.
