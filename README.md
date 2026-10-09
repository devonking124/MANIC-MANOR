# M4N1C M4N0R

A WebXR soul-battle game in a single `index.html`: Undertale/Deltarune-style turn-based
boss fights built for the Meta Quest 3S, with grab-and-pull locomotion. Everything
(models, textures, pixel font, SFX, fallback music) is procedural; three.js r160 from
unpkg is the only dependency. The soundtrack ships next to it in `music_pack.js`.

Four bosses, picked from the SAVE FILES screen, and a shop between them:

- **FILE 1 - THE MANOR**: THE PROPRIETOR, a haunted mansion in a butler's body.
- **FILE 2 - THE SERVER**: SCR1PT_K1DD1E, a smug script kiddie who copy-pastes other
  people's attacks (including the Proprietor's). Encrypted until you finish boss 1 with
  either ending (or turn on Settings > UNLOCK ALL BOSSES). Finishing boss 1 also gets
  hijacked straight into boss 2 ("lol wait dont leave yet"); RETURN TO TITLE skips it.
- **FILE 3 - THE BIG TOP**: GRANDIOSO, Ringmaster of the Sky, a 7 m showman on stilts
  whose circus floats above the clouds on hot-air balloons. Shows a torn SOLD OUT ticket
  until you finish boss 2 with either ending. After boss 2's ending an ADMIT ONE ticket
  flutters down: poke it to fly into the big top (YES / NO skips it). Bosses 1 and 2 you
  spared watch from the front row and help once per fight (Settings > AUDIENCE ASSISTS
  turns that off); a boss you defeated leaves an empty seat under a dusty spotlight.
- **FILE 4 - THE FRONTIER**: SUNDOWN, the Last Outlaw, a 6 m gunslinger whose face is
  the sun. He shot his town's clock at high noon a hundred years ago so the day would
  never end. Shows a WANTED poster with a torn-off reward until you finish boss 3. After
  boss 3's ending a tumbleweed rolls by and a WANTED poster with your soul on it blows
  into your hands: grab it (or poke it) to ride out (YES / NO skips it). The fight runs
  from HIGH NOON through GOLDEN HOUR to MIDNIGHT and ends in a duel in the street.
- **KNOBS' SHOP**: a late-night record shop floating in the void, run by KNOBS. Reached
  from the door under the save files ("open late") or "* Swing by the shop?" after any
  fight.

Each boss also carries one ★ item you can't buy anywhere (HOUSE KEY, CTRL+Z, BOUQUET,
POCKET WATCH) that only works on that boss.

## Run it on Quest 3S

1. Host `index.html` and `music_pack.js` side by side on any HTTPS server (GitHub Pages
   works: Settings > Pages > deploy from this branch). WebXR requires HTTPS.
2. Open the page in the Quest Browser, press **ENTER VR**, then START and pick a file.

The music is built in: `music_pack.js` defines `window.MANIC_MUSIC` with all five tracks
(four bosses + the shop) as data URIs, so there are no file prompts. Each track is only
decoded when its scene starts ("tuning instruments..." while it decodes), and only the
current boss track and the shop track are kept decoded. Priority per track: a file you
pick in Settings > MUSIC > REPLACE this session > `MUSIC_DATA_URI_*` / `MUSIC_URL_*`
overrides at the top of the script > `music_pack.js` > a copy in IndexedDB (saved by
REPLACE) > the boss's procedural fallback. Without `music_pack.js` the game still runs.

Desktop testing: press **PLAY ON DESKTOP**.

Progress (which bosses you beat and how, GOLD, EXP, LV, upgrades, bag, cosmetics) and
settings are saved in `localStorage` under `manicManor.save.v2`. Older saves load with
defaults (and get paid the first-clear rewards for bosses already beaten). If storage is
blocked, the game still runs and progress lasts for the session.

## GOLD, EXP and LV

Finishing a boss pays EXP and GOLD (the first clear of each route in full, replays 50%;
SPARE pays no EXP but 25% more gold). You also find gold during fights: 1 G every 15
grazes and 2 G per critical FIGHT; a GAME OVER or quitting keeps half of what you found.
LV 2-6 at 100 / 300 / 650 / 1100 / 1700 EXP, each +4 max HP and +5% damage. SOUL STATS in
Settings shows everything. **PURIST MODE** (Settings > GAME) turns off every shop and LV
bonus for a plain run.

| Boss | FIGHT | SPARE |
|---|---|---|
| THE PROPRIETOR | 300 EXP, 120 G | 150 G |
| SCR1PT_K1DD1E | 420 EXP, 69 G | 86 G |
| GRANDIOSO | 600 EXP, 300 G | 375 G |
| SUNDOWN | 800 EXP, 400 G | 500 G |

## KNOBS' SHOP

Walk in (room-scale or the left stick, snap turn on the right; no flying), pick an item
off the shelf with grip, set it on the glowing pad on the counter and poke **BUY**. Tapes
are permanent upgrades with tiers (BASS BOOST, SHARP EDIT, PADDED MIX, GRAZE MAGNET,
AFTERIMAGE, GRIP TAPE, B-SIDE POCKETS, SECOND WIND, METRONOME); consumables go in your BAG
(ITEM > BAG in any fight); SOUL PINs are cosmetics. **HAGGLE** (tap the counter on 8
beats for a discount), **SELL**, **TALK** (new topics unlock as you beat bosses) and
**SOUND TEST** (KNOBS' jukebox plays every track you've heard). The shop beat loops with a
crossfade, the music is muffled behind the closed door and ducks under KNOBS' voice.

## Controls

| | VR | Desktop |
|---|---|---|
| Fly | hold grip and pull the air, release to fling | WASD + Q/E, Space to dash |
| Turn | right stick (snap or smooth; the sticks never move you) | mouse look |
| Menus, pop-up [X], [ACCEPT] | poke with either hand, or point and pull the trigger | crosshair + click, keys 1-9 |
| Move a pop-up out of the way | grip it with either hand, drag, let go | - |
| FIGHT (boss 1) | swing through the ring when the line hits center (it stops where you strike) | click on the beat |
| TYPE ATTACK (boss 2 FIGHT) | punch each lit key on its beat | press that letter on its beat |
| DANCE | punch the markers on the beat | arrow keys |
| YELLOW soul | hold trigger to shoot where your hand points | hold click (or F) |
| CYAN soul | hold grip and drag (the heart follows your hand); it turns at junctions by itself | WASD / arrow keys |
| TEACH HIM | grip-grab the code blocks and drop them into the slots, then RUN | click two blocks to swap |
| JUGGLE STRIKE (boss 3 FIGHT) | catch each pin (grip) as it reaches your hand on its beat, then really throw it at him | click as each pin arrives |
| PINK soul (trapeze) | grip a bar when it glows, let go to fling; in the air you drift toward the nearest bar ahead | Space lets go (with a hop), catching is automatic; WASD steer, A/D shimmy |
| APPLAUD / JUGGLE / TAKE A BOW | clap on the beat / toss and catch / bow for real | Space / click / hold S |
| QUICK DRAW (boss 4 FIGHT) | grip the revolver on your dominant hip to draw, trigger to fire; shoot each target as its ring closes | the gun is in hand: click each target |
| TIP YOUR HAT | raise a hand to the brim of an invisible hat (above your head) and hold it a beat | hold Space |
| SPIN THE REVOLVER | draw, then roll your wrist around and around for a whole bar (let go and you drop it) | hold Space |
| POINT AT THE SUNSET | aim a controller at the sun for a beat | look at the sun, hold Space |
| HOLSTER | draw, slide the gun back into the holster and let go, then leave it there for his whole attack | Space (R draws) |
| Green dynamite | grip a green-glowing stick and throw it back at him | look at it and click |
| The final duel | draw and shoot on the third bell toll (or keep it holstered to spare him) | Space / click on the third toll |
| Shop | walk with the left stick or your feet, grip to pick up, poke BUY | WASD + mouse, click |
| Pause | B / Y | Esc |

Debug keys (desktop): `H` hitboxes, `G` god mode, `N` next phase (in the last phase:
boss HP 1 and mercy 100%), `B` beat grid + metronome with the section name, `[` `]` nudge
the music offset by 5 ms, `P` performance stats (also shows `renderer.info.memory`).
While `P`, `G` or `H` is on, `renderer.info.memory` is also logged to the console at every
fight start and every return to the title. Leaving a fight frees the boss model, its
attack kits and its UI, so those numbers stay flat from fight to fight.

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
beats while the music crossfades over one bar into the interlude, which runs in
**free-time mode** (spawns follow note onsets detected from the music). Finale (below 25%
HP or 75% mercy): a drumroll swells over one bar, then the finale hits on the next bar
line with the lights blasting on (a 0.5 s fade in Reduced flashing mode). The procedural
fallback is a chromatic F-minor calliope waltz in 3/4 at 89 BPM on the same section names.

**Boss 4, "Yeed Your Last Haw":** 89.5 BPM (beat 0.6704 s, bar 2.682 s) in 4/4 with a
double-time gallop (a boom on every beat, a chick on every half beat: rapid spawns use
the half beat). The beat grid sits on the boom: `OUTLAW_OFFSET` = 0.02 s (auto-sync
refines it by up to 60 ms; `[` `]` nudge it, and every section below moves with it). The
brief's 0.41 s offset lands on the off-beat chick, which would put every downbeat on the
"and". The song counts 186 beats; every boundary below is a beat number in
`OUTLAW_BEATS`:

| Section | Beats | Time (s) | Use |
|---|---|---|---|
| Standoff | 0-8 | 0.0-5.38 | the intro (near silence, wind) |
| A1 + A2 | 8-72 | 5.38-48.29 | phase 1 loop, HIGH NOON; A2 (from 26.84 s) uses the harder variants |
| GAP 1 | 72-74 | 48.29-49.63 | THE STARE-DOWN: everything freezes, then every star fires when B hits |
| B | 74-134 | 49.63-89.85 | phase 2 loop, GOLDEN HOUR |
| GAP 2 | 136-138 | 91.19-92.53 | SUNSET: the sun drops below the horizon, his face turns to the moon |
| C | 138-170 | 92.53-113.99 | phase 3 loop, MIDNIGHT (the finale) |
| Outro | 170-186 | 113.99-124.71 | the final duel, then both endings |

Each gap is half a bar, so B and C sit half a bar off A's bar lines. Their sections are
**exact** (used as is, never snapped to A's grid), and the clock starts an exact section
on a bar line of the running count. The transitions seek, on a bar line, to two beats
before a gap, so the hit after the silence lands exactly on the next bar line. The duel
seeks to the outro on the next bar line. The procedural fallback is a D-major western at
89.5 BPM in the same 186-beat shape (a boom-chick gallop bass, a twangy Karplus-Strong
lead with little pitch bends, a whistled melody, a whip crack every 4 bars, silent gaps
and a quiet outro).

**The shop, "made this beat to practice mastering":** 106 BPM, bar 0 at 1.656 s; it
loops 6.184-87.693 s (bars 2-38) with a 0.15 s crossfade at the seam.

## SUNDOWN, the Last Outlaw

- **The frontier.** You stand in the corral on top of your own sandstone mesa, and he
  stands on his across a narrow canyon (solid ground under your feet; the canyon floor is
  7.5 m below). Around you: banded mesas, buttes and hoodoos fading into the haze,
  saguaros and prickly pears, the ghost town of Ghost Creek with its stopped clock tower,
  a railroad, a windmill, tumbleweeds and dust devils.
- **The day/night arc.** HIGH NOON (a blinding white-gold sun face, bleached sky, short
  hard shadows, a hawk circling), GOLDEN HOUR (a deep orange sun, an amber-to-rose sky,
  long shadows, the shells on his gunbelts glowing) and MIDNIGHT (his face a cratered
  moon, the milky way, shooting stars, fireflies, the hat gone). The battle box is a fence
  of weathered beams and rope with iron brackets and two lanterns: dust shakes off it on
  every beat and the rope creaks taut on every bar.
- **Attacks** (all on the beat or the half-beat gallop, telegraphed at least a beat ahead,
  silver bullets glint a beat before they fire): TUMBLEWEED STAMPEDE, FAN THE HAMMER,
  CACTUS FIELD (BLUE / ORANGE needle volleys by the bar), WANTED (posters with your soul
  on them, the bounty counting up), TRAIN HEIST (an 8-bar set piece on a flatcar: the box
  and you never move or accelerate, only the canyon scrolls past; duck the tunnel beams,
  which always stay above ~60% of your standing height), LASSO LOOPS (a gentle tug back
  to the middle if you're caught, never damage), DYNAMITE (green sticks can be thrown
  back for 60 damage), GHOST POSSE, MOONLIGHT RICOCHET (every bounce path drawn a beat
  ahead, at most 3 bounces) and YEED YOUR LAST HAW (the midnight medley; the clock tower
  starts ticking again and the last 4 bars slow down).
- **Mercy route:** TIP YOUR HAT and SPIN THE REVOLVER (any time), SHARE A CAMPFIRE STORY
  (golden hour, 30% TP: heals 10 and he goes quiet, so his next attack is slower) and
  POINT AT THE SUNSET (golden hour), then HOLSTER at midnight (only after the story): keep
  your gun holstered through his whole attack. At 100% his name turns yellow and the moon
  slowly brightens toward dawn. Spare him from MERCY or just let the duel come.
- **Cameos:** every boss you spared sends a telegram in the golden hour, and helps once:
  THE PROPRIETOR's chandelier pins a lasso, SCR1PT_K1DD1E's antivirus.exe defuses a
  dynamite volley, GRANDIOSO's spotlight reveals a ricochet path early (Settings >
  AUDIENCE ASSISTS). At the SPARE ending they sit around the campfire with him at dawn.
- **The final duel** (at 4% HP or 100% mercy): the box clears into a dusty street at
  midnight, the clock tower bell tolls every 2 beats, and you draw on the third toll.
  Too early: he sidesteps (3 damage); too late or wide: he grazes you (4 damage); either
  restarts the count, and your HP never drops below 1 there.
