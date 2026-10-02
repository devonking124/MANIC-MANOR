# M4N1C M4N0R

A WebXR soul-battle boss fight in a single `index.html`: Undertale/Deltarune-style turns
against THE PROPRIETOR, a haunted mansion in a butler's body, built for the Meta Quest 3S
with grab-and-pull locomotion. Everything (models, textures, pixel font, SFX, fallback
music) is procedural; three.js r160 from unpkg is the only dependency.

## Run it on Quest 3S

1. Host `index.html` on any HTTPS server (GitHub Pages works: Settings > Pages > deploy
   from this branch). WebXR requires HTTPS.
2. Open the page in the Quest Browser.
3. Press **LOAD MUSIC (.mp3)** and pick the "M4N1C M4N0R" MP3 (for example from the
   headset's Downloads folder). Without it, a procedural chiptune plays instead.
4. Press **ENTER VR**, then START in the world.

To skip step 3, either paste a base64 data URI into `MUSIC_DATA_URI`, or upload the MP3
next to `index.html` and set `MUSIC_URL` (both constants are at the top of the script).

Desktop testing: press **PLAY ON DESKTOP**.

## Controls

| | VR | Desktop |
|---|---|---|
| Fly | hold grip and pull the air, release to fling | WASD + Q/E, Space to dash |
| Glide / turn | left stick glide, right stick snap turn and up/down | mouse look |
| Menus | poke buttons, or point and pull the trigger | crosshair + click, keys 1-4 |
| FIGHT | swing through the ring when the line hits center | click on the beat |
| DANCE | punch the markers on the beat | arrow keys |
| Pause | B / Y | Esc |

Debug keys (desktop): `H` hitboxes, `G` god mode, `N` next phase, `B` beat grid +
metronome, `[` `]` nudge the music offset by 5 ms, `P` performance stats.

## Music sync

The supplied MP3 measures **203.00 BPM** (beat 0.2956 s, bar 1.1823 s) with the first
downbeat at 0.038 s, flat to within 1 ms across the whole track. 136 BPM is its 2:3
sub-harmonic and drifts off the kick. The sections fall on bar lines: A = bars 1-48
(1.22-57.97 s), B = bars 49-64 (57.97-76.89 s, with the dip in bar 64), C = bars 65-90
(76.89 s to the end). `CONFIG.AUTO_SYNC` refines the offset from the decoded audio at
load time (it finds 0.041 s for this file).
