# M4N1C M4N0R

A WebXR soul-battle game in a single `index.html`: Undertale/Deltarune-style turn-based
boss fights built for the Meta Quest 3S, with grab-and-pull locomotion. Everything
(models, textures, pixel font, SFX, fallback music) is procedural; three.js r160 from
unpkg is the only dependency.

Three bosses, picked from the SAVE FILES screen:

- **FILE 1 - THE MANOR**: THE PROPRIETOR, a haunted mansion in a butler's body.
- **FILE 2 - THE SERVER**: SCR1PT_K1DD1E, a smug script kiddie who copy-pastes other
  people's attacks (including the Proprietor's). Encrypted until you finish boss 1 with
  either ending (or turn on Settings > UNLOCK ALL BOSSES). Finishing boss 1 also gets
  hijacked straight into boss 2 ("lol wait dont leave yet"); RETURN TO TITLE skips it.
- **FILE 3 - THE BIG TOP**: GRANDIOSO, Ringmaster of the Sky, a 7 m showman on stilts
  whose circus floats above the clouds on hot-air balloons. Shows a torn SOLD OUT ticket
  until you finish boss 2 with either ending. After boss 2's ending an ADMIT ONE ticket
  flutters down: poke it to fly into the big top (RETURN TO TITLE skips it). Bosses 1 and
  2 you spared watch from the front row and help once per fight (Settings > AUDIENCE
  ASSISTS turns that off); a boss you defeated leaves an empty seat under a dusty
  spotlight.

## Run it on Quest 3S

1. Host `index.html` on any HTTPS server (GitHub Pages works: Settings > Pages > deploy
   from this branch). WebXR requires HTTPS.
2. Open the page in the Quest Browser.
3. Press **LOAD BOSS 1 MUSIC** and pick "M4N1C M4N0R", **LOAD BOSS 2 MUSIC** and pick
   "SCR1PT_KIDDIE", and **LOAD BOSS 3 MUSIC** and pick "THE CIRCUS IN THE SKY" (for
   example from the headset's Downloads folder). A green check appears next to each
   loaded track. Without a file, that boss plays its own procedural fallback track.
4. Press **ENTER VR**, then START and pick a file.

To skip step 3, paste a base64 data URI into `MUSIC_DATA_URI_PROPRIETOR` /
`MUSIC_DATA_URI_KIDDIE` / `MUSIC_DATA_URI_RINGMASTER`, or upload the MP3s next to
`index.html` and set `MUSIC_URL_PROPRIETOR` / `MUSIC_URL_KIDDIE` / `MUSIC_URL_RINGMASTER`
(all at the top of the script).

Desktop testing: press **PLAY ON DESKTOP**.

Progress (which bosses you beat, and how) and settings are saved in `localStorage`
under `manicManor.save.v2`. If storage is blocked, the game still runs and progress
lasts for the session.

## Controls

| | VR | Desktop |
|---|---|---|
| Fly | hold grip and pull the air, release to fling | WASD + Q/E, Space to dash |
| Glide / turn | left stick glide, right stick snap turn and up/down | mouse look |
| Menus, pop-up [X], [ACCEPT] | poke with either hand, or point and pull the trigger | crosshair + click, keys 1-9 |
| Move a pop-up out of the way | grip it with either hand, drag, let go | - |
| FIGHT (boss 1) | swing through the ring when the line hits center | click on the beat |
| TYPE ATTACK (boss 2 FIGHT) | punch each lit key on its beat | press that letter on its beat |
| DANCE | punch the markers on the beat | arrow keys |
| YELLOW soul | hold trigger to shoot where your hand points | hold click (or F) |
| CYAN soul | thumbstick, or hold grip and drag (the heart follows your hand); it turns at junctions by itself | WASD / arrow keys |
| TEACH HIM | grip-grab the code blocks and drop them into the slots, then RUN | click two blocks to swap |
| JUGGLE STRIKE (boss 3 FIGHT) | catch each pin (grip) as it reaches your hand on its beat, then really throw it at him | click as each pin arrives |
| PINK soul (trapeze) | grip a bar when it glows, let go to fling; left stick steers in the air | Space lets go (with a hop), catching is automatic; WASD steer, A/D shimmy |
| APPLAUD | clap your controllers together on every beat for 2 bars | Space / click on the beat |
| JUGGLE | grab a ball, toss it up, catch it again (6 catches, low gravity) | click as each ball reaches your hand |
| TAKE A BOW | face him and bow (head below 70% of your height; seated 80%) | hold S or Space |
| Pause | B / Y | Esc |

Debug keys (desktop): `H` hitboxes, `G` god mode, `N` next phase, `B` beat grid +
metronome, `[` `]` nudge the music offset by 5 ms, `P` performance stats (also shows
`renderer.info.memory`). While `P`, `G` or `H` is on, `renderer.info.memory` is also logged
to the console at every fight start and every return to the title. Leaving a fight frees
the boss model, its attack kits and its UI, so those numbers stay flat from fight to fight.

## Music sync

**Boss 1, "M4N1C M4N0R":** 203.00 BPM (beat 0.2956 s, bar 1.1823 s), first downbeat at
0.038 s, flat to within 1 ms across the whole track. 136 BPM is its 2:3 sub-harmonic and
drifts off the kick. Sections: A = bars 1-48 (1.22-57.97 s), B = bars 49-64
(57.97-76.89 s), C = bars 65-90 (76.89 s to the end). Phase changes use a tape-stop.

**Boss 2, "SCR1PT_KIDDIE":** 119.50 BPM (beat 0.5021 s, bar 2.0084 s), first downbeat at
0.349 s (auto-sync finds 0.354 s). The 117.5 BPM / 0.14 s grid from the design brief
drifts about 4.5 beats over the song, so it would miss the kick. The brief's section
times map onto these bar lines (all tunable in `KIDDIE_MUSIC`):

| Section | Bars | Time (s) | Use |
|---|---|---|---|
| A | 0-16 | 0.35-34.49 | phase 1 loop |
| GAP 1 | 17 | 34.49-36.50 | CONNECTION LOST: bullets freeze, explode when B1 hits |
| B1 / B2 | 18-38 / 39-49 | 36.50-78.68 / 78.68-100.77 | phase 2 loop; B2 = hardest variants |
| GAP 2 | 50 | 100.77-101.52 | FATAL ERROR |
| C | | 101.52-108.80 | scripted REBOOT |
| GAP 3 | 54 | 108.80-109.56 | `sudo su` |
| D | 55-66 loop | 109.56-134.91 | phase 3 (ADM1N_K1DD1E) |

Phase changes for boss 2 seek into the song's own silences: on the next bar line the song
jumps to one bar before GAP 1 (or GAP 2) and the gap carries the transition. The
procedural fallback (117.5 BPM, D minor, bitcrushed 16th arp) follows the same bar map,
so the transitions work without the MP3 too.

**Boss 3, "THE CIRCUS IN THE SKY":** measured at 90.00 BPM (beat 0.6667 s), first
downbeat at 0.032 s, with the phrases and section starts on 4/4 bar lines (bar k starts
at 0.032 + 2.6667 k s). The brief's 89.1 BPM / 0.07 s grid drifts about 2.5 beats over the
song, and its 3/4 bar lines miss the section starts. `RINGMASTER_MUSIC.beatsPerBar` is
the single meter constant (set it to 3 to try the waltz grid). Sections (tunable):

| Section | Bars | Time (s) | Use |
|---|---|---|---|
| Overture | 0-7 | 0.03-21.37 | plays once under the intro and the first turn |
| A + A' | 8-31 | 21.37-85.37 | phase 1 loop ("THE OPENING ACT"); A' (from 53.37 s) uses the harder variants |
| Interlude | 32-39 | 85.37-106.70 | phase 2 loop ("LIGHTS OUT"), free-time mode |
| Finale | 40-55 | 106.70-149.37 | phase 3 loop ("THE GRAND FINALE") |
| Outro | 56-63 | 149.37 to the end | under both endings, never combat |

Lights out (below 55% HP or 40% mercy): on the next bar line the bulbs pop off every 2
beats while the music crossfades over one bar into the interlude. The interlude runs in
**free-time mode**: spawns follow note onsets detected from the music (spectral flux on an
AnalyserNode, adaptive threshold, at least 250 ms apart) instead of the beat grid, and
bullets born on an onset stay harmless for 350 ms. Finale (below 25% HP or 75% mercy): a
drumroll swells over one bar, then the finale hits on the next bar line with the lights
blasting on (a 0.5 s fade in Reduced flashing mode). Every bar-snapped value uses
`beatsPerBar`, which every boss definition now carries (bosses 1 and 2 are 4).

The procedural fallback is a chromatic F-minor calliope waltz in 3/4 at 89 BPM (steam
organ lead, oom-pah-pah, glockenspiel) with a quiet music-box interlude and a brighter
A-major finale, laid out on the same section names so every transition works without
the MP3.
