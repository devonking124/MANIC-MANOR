# SoulRift

*(formerly M4N1C M4N0R; boss 1's song keeps that name.)*

A WebXR soul-battle game in a single `index.html`: Undertale/Deltarune-style turn-based
boss fights built for the Meta Quest 3S, with grab-and-pull locomotion. Everything
(models, textures, pixel font, SFX, fallback music) is procedural; three.js r160 from
unpkg is the only dependency. The soundtrack ships next to it in `music_pack.js`.

The title is THE RIFT: you stand on a floating island in deep space, in front of a tear
of light where the worlds leak through, and every fight starts by falling into it.

START opens the mode screen. **STORY MODE** is the main game: an opening tells how the
RIFT began (a hundred years ago SUNDOWN shot his town's clock at high noon so his day
would never end, and the bullet tore time; everything else that refuses to end fell into
the tear) and why you face each keeper, then the SAVE FILES screen. The opening plays
once (SKIP or Esc jumps past it); after that STORY MODE goes straight to SAVE FILES.
**FREE PLAY** is locked until you beat SUNDOWN, and has nothing in it yet.

Four bosses, picked from the SAVE FILES screen (a card per boss: the boss's real model in
front of its animated world, in a frame of its world's colours), and a shop between them:

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

Each file is harder than the one before. GRANDIOSO and SUNDOWN have bigger HP bars, hit
harder and run their acts hotter, and between their acts' own beats they aim at you: a
gold line (GRANDIOSO's knives) or an orange one (SUNDOWN's revolver) shows each shot a
beat before it flies at where your SOUL is. Standing still won't do; keep moving.

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

The shop stands on a floating island of dark rock in a violet void: a nebula and a far
galaxy overhead, the horizon one long glowing waveform that dances to the music, records,
tapes and old speakers drifting past. Its walls are stacked speaker cabinets (the cones pump
with the bass, and the big one on the roof sends a ring out on every bar); there are display
windows, a striped awning, the neon in a ring of chasing bulbs and a street lamp with a
gramophone horn. Inside: lamplight pooling under the pendants, fairy lights, shelves of
real records, a bubble-tube jukebox, a reel-to-reel, a brass cash register. KNOBS' face is
a glowing screen in his speaker head; he waves you in, points at prices, gives a thumbs-up
when the till rings, shrugs, and taps the counter with you when you haggle.

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
| Fly | hold grip and pull the air, release to fling (Settings > COMFORT > SOUL DRIFT OFF: you stop where you let go) | WASD + Q/E, Space to dash (SOUL DRIFT OFF: a short dash that stops dead) |
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
| Skip the opening | poke SKIP | Esc, or click SKIP |
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

**The secret, "_____":** 176.0 BPM (beat 0.3409 s, bar 1.3636 s) with a heavy half-time
feel at 88 BPM, first beat at 0.016 s (`SECRET_OFFSET`), G minor, 2:14.4. The design
brief's 172.3 BPM / 0.07 s grid slides off the kick within a few bars: folding the song's
onsets over its whole length scores 176.0 BPM at 2.35x the on-beat/off-beat contrast of
172.3 (1.28x). Every section starts on a bar line of the 176 grid (beat numbers in
`SECRET_BEATS`):

| Section | Beats | Time (s) | Use |
|---|---|---|---|
| VOID | 0-32 | 0.0-10.92 | the fall through the black hole |
| IMPACT | 32 | 10.92 | landing in THE NULL; its eye opens |
| BUILD | 32-64 | 10.92-21.83 | the intro and the first turn (loops from beat 36) |
| (gap) | 64-68 | 21.83-23.19 | near silence before the drop |
| DROP | 68-132 | 23.19-45.01 | phase 1 loop; ends on the big hit at 45.01 s |
| BREAKDOWN / REBUILD | 132-196 | 45.01-66.83 | phase 2 loop (the rebuild's hit is at 55.92 s) |
| PEAK | 196-260 | 66.83-88.65 | phase 3 loop, the loudest point of the soundtrack |
| SECOND WAVE | 260-324 | 88.65-110.47 | phase 4 loop |
| FINAL HIT | 324 | 110.47 | the last blow (either ending) |
| OUTRO | 324-392 | 110.47 to the end | under the endings |

The procedural fallback is drum and bass in G minor at 176 BPM in the same 392-beat shape
(a broken breakbeat, a detuned reese bass, dissonant stabs, bit-crushed stutter fills, a
sub rumble at both ends, a hollow breakdown and a peak), so every seek lands where the
real song's would.

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

## _____ (the secret boss; spoilers)

Hidden inside boss 2.

- **The BLOTCHED TAPE.** Once you've beaten SCR1PT_K1DD1E (either ending), a cassette
  nobody stocked turns up in KNOBS' SHOP: half-hidden in a crate on two dead speakers, in
  the dark nook between the end of the counter and the record wall. Black ink crawls over
  it, its reels turn backwards, the corner's a little darker, and the shop's music
  bit-crushes and wobbles in pitch as your hand gets close. Its tag reads `BL█TCH_D T█PE`,
  `???`, `?? G`; it costs 1 G. KNOBS stops nodding to the beat while it's out of its crate
  ("...where'd that come from. that's not one of mine."). You can carry one, and another
  turns up once it's used.
- **In any other fight** (ITEM > BAG) it does nothing, isn't used up and costs no turn:
  "It doesn't like it here."
- **In boss 2's fight** it corrupts his MERCY: -20 at once, -10 at the start of every
  turn, every ACT garbled and -15, every FIGHT hit -5, and no SPARE while it's negative.
  The bar runs left past zero in a black-and-red glitch, down to -100%. As it falls the
  fight comes apart: at -25 the box's edges jitter and the music wobbles +/-1.5% in pitch;
  at -50 his chat glitches, he gets scared and holes open in the hall; at -75 bullets
  flicker into black outlines, the menu buttons drift and words lose their letters. End
  the fight normally first and the tape is simply used up. A retry keeps the MERCY it had
  fallen to. Reduced flashing slows or softens every one of these.
- The pitch wobble is a slow vibrato on the music bus (a modulated delay), not a change of
  playback rate, so the beat clock and every attack stay exactly on the beat.
- **At -100 the black hole opens** (a script on the audio clock, so a pause holds it):
  the music cuts mid-bar, every bullet freezes and the HUD drains letter by letter into
  underscores. "wait." ... "thats not my code." The box peels apart into floating pixels
  and a single black pixel hums in the hall. Over 3 s it opens into an event horizon: a
  black sphere, a blazing photon ring, an accretion disk (white to cyan to magenta,
  Doppler-bright on the side coming toward you) and a lens that shows the stars warped
  round it. Then everything goes in, stretched toward it: the frozen bullets, the menus,
  the HP bar, his chat ("NONONONO"), his CRT, him, the hall itself, piece by piece. You
  never move: the world goes in around you, the vignette closes, your SOUL stretches
  toward it last, and it all fades to black over 1.5 s.
- **The fall** is the secret track from 0:00: darkness, a few motes drifting slowly up
  past you (the only motion), a faint grid sphere that stays put, and deleted words
  floating by ("deleted", "unsaved", "0 bytes", "not responding"...). The song's IMPACT at
  0:11 is scheduled on the audio clock, and THE NULL appears exactly on it: an endless
  white wireframe grid sagging toward a black hole that hangs huge in the sky, over a
  procedural starfield lensed round it (the bending happens in the sky's own shader, so
  there's an Einstein ring for almost nothing). Every shader is compiled, and drawn
  unseen for a few frames, while it's dark, so nothing hitches on the reveal.
- **Your ally.** SCR1PT_K1DD1E fell in too. In THE NULL he fights beside you: the first
  (and only) co-op fight, on a party system any fight can use. He floats about 1.5 m to
  your side inside the box, sitting on a cracked, half-deleted mini CRT, his hoodie's
  terminals trying to compile his first real program. The status row under the box (and a
  panel under your HP on your left wrist) shows SOUL and K1DD1E (60 HP). He dodges, a beat
  late and not always the right way, and a bullet that reaches him hurts him. At 0 HP
  he's DOWN (washed out like a ghost, lagging, NOT RESPONDING over his head) until he's
  patched; he can't die, and only you can lose.
- **His turn** comes after yours, from his own menu in his colours:
  - **H4CK**: punch his 4 keys on the beat (his TYPE ATTACK); half your FIGHT's damage,
    straight through its armour.
  - **F1REWALL**: a hex shield orbits your SOUL through the next attack and blocks its
    first 3 hits.
  - **P4TCH**: heals you 18 HP; when he's DOWN it's the only thing he can run, and it
    brings him back at 50%.
  - **D3BUG**: a weak seam glows on its body; your next FIGHT does x2.
- **TP is shared.** The X-ACTION **CTRL+ALT+DEL** (100% TP, him not DOWN): he types
  like mad, three giant keycaps (CTRL, ALT, DEL) fall into reach, and you punch each on
  three consecutive half-time beats (desktop: click on them) for 4% of its max HP a key,
  12% for all three. It uses his turn too.
- During attacks he shouts callouts ("LEFT LEFT LEFT", "dont touch the outlines!!"), and
  about once an attack he shoots the bullet nearest to you down with a little yellow star.
  His compile bar fills a step every phase, but only while he's never been DOWN for more
  than 2 turns in a row (then: BUILD FAILED).
- **_____ itself.** What's left after things end: deleted files, unsaved progress, closed
  tabs, erased words. Every other boss is afraid of endings; this is what they were afraid
  of. It doesn't hate you. It just erases. About 30 m tall, standing far behind the box: a
  hunched silhouette of absolute black with a crisp white outline (an inverted hull, the
  same thin line at any distance), its edges crumbling into square pixels that drift off
  and come back. Holes drift open and shut through it, white-rimmed, and through them you
  see the lensed stars behind it: it looks cut out of the sky. Its face is one enormous
  empty text field; the eye is a text cursor that blinks on the half-time pulse (never out
  of rhythm) and slides to follow your SOUL, the mouth a long underscore that stretches when
  it speaks. Its hands are a marquee (marching ants) and a lasso cursor. Pieces of the other
  bosses orbit it, inverted and half erased: a chandelier crystal, a strip of tent canvas,
  SUNDOWN's hat and one of SCR1PT_K1DD1E's keyboards ("hey... thats MINE"). When it moves it
  leaves datamosh smears; between attacks it types words into its face and deletes them.
  Its name on the HUD is `_____`, flickering, letters almost appearing.
- **Its voice** types into its face and into a dialogue box shaped like the same empty
  field, at eye level, with letters missing: `Y_U _ER_ N_T S_PP_S_D T_ B_ H_R_.`,
  `_V_RYTH_NG _NDS.`, `I _M WH_T C_M_S _FT_R.` No blip, just silence and a faint
  keyboard-delete "tk" a moment behind each letter.
- **Stats.** HP 6666, ATK 22, DEF 10. Its hits do 7-9 (9-11 in phase 4). It's enormous, so
  your FIGHT does x1.5 here (H4CK too: still half of yours), x0.75 through its armour in
  phases 3-4 (H4CK and CTRL+ALT+DEL go straight through). Only the finishing move can take
  its last HP. Grazes give 50% more TP. Easy and Purist scale it like every boss.
- **THE NULL's box is broken**: its 12 edges are 36 floating fragments that realign into a
  whole box on every bar line and drift a few cm apart in between. The remains of every
  arena drift round it, low-poly and inverted (manor wallpaper, hex characters, circus
  bunting, desert rocks), off to the sides and behind you, never between you and it; some
  orbit the black hole and stretch thin as they fall in.
- **Phases, on the song:**

  | Phase | Starts | Music | Look |
  |---|---|---|---|
  | 1 NULL POINTER | the first attack | the drop (the first attack waits for the gap, so the drop lands with it) | the black silhouette |
  | 2 ERASURE | under 70% | seeks one bar before the 0:45 hit; BREAKDOWN into REBUILD | cracks of white light split across it |
  | 3 SEGFAULT | under 40% | one bar before the 1:06.6 drop; the PEAK | it inverts (0.3 s, Reduced 1 s): white body, black outline, and the whole world with it; white bullets turn black, telegraphs draw dark |
  | 4 _____ | under 15% | a crossfade into the second wave on the next bar | it unravels into wireframe, vertices drifting; its text field is the only solid thing left |

  Each change lands on its hit (a soft beat of light, Reduced: a slow fade).
- **Its attacks** (every telegraph 2 beats or more, in a sickly `#c8ff00`; heavy hits on
  the half-time pulse, streams on the beat; each escape is checked reachable at a
  comfortable speed):
  - **MARCHING ANTS**: its marquee drags 3D selection boxes round the box; their dashed
    outlines march for 2 beats, then everything inside is deleted (a cut, a beat of missing
    texture). Later waves add inverse selections (in the telegraph colour): only inside is
    safe.
  - **MISSING TEXTURE**: the floor flips into pink-and-black checker columns that shoot up
    in rows on the beat. The ripple's start and direction show 2 beats ahead, each row lights
    2 beats before it erupts, and the cells that stay dark are a winding gap.
  - **UNDO HISTORY**: it records your SOUL's path; hollow inverted hearts tagged `-2 BARS`
    (and later `-1 BAR`) retrace exactly where you were, leaving dotted trails, with a
    whisper. Stay still or move in loops and your past catches you.
  - **DELETED_K1DD1E**: the lasso pulls him into a marching-ants cage across the box; he
    loses pixels and HP. Get there and punch the cage on the half-time beat 4 times while
    erasers sweep the box. Saved: +10% TP and a fist bump (touch his tiny glove: a soft
    pulse), "...ok. ok. thanks. dont tell anyone i screamed". Too slow: he's DOWN.
  - **ECHOES**: corrupted re-runs of the other bosses' attacks, their own pattern classes
    with the corrupt flag, at this song's tempo: THE PROPRIETOR's CHANDELIER RAIN as black
    crystals, GRANDIOSO's RING OF FIRE as hollow black rings (its hole picking and beat plan,
    without the big top), SUNDOWN's FAN THE HAMMER fired from its hands, the stars smearing
    pixels. Each deletes itself mid-attack.
  - **STACK TRACE** (REBUILD only, the song seeks to it): curtains of falling error text pour
    in and sweep the box; the one column that reads OK is the gap. Every 4 beats, then 2,
    then every beat in the last bar.
  - **EVENT HORIZON**: a mini black hole in the middle of the box pulls your SOUL (2 m/s^2 at
    most; streaming specks and a rising tone show it; it only ever moves you, never turns
    you). Bullets fall into real orbits round it (each orbit shows a thin dashed ring 2 beats
    ahead, with a gap) and spiral in on the half-time pulse.
  - **MIRROR BUFFER**: a shimmering, harmless sheet sweeps the box every bar; every bullet
    has a mirrored copy across it, so the safe space keeps changing shape.
  - **FRAME SKIP**: the bullets jump once a beat; between jumps they're dim, and a ghost
    ring shows where each will land a beat ahead. Only the bullets skip.
  - **DELETE UI**: its marquee closes round your HUD for 2 beats and deletes it: your HP, TP
    and menu are gone. SCR1PT_K1DD1E restores each while you hold its panel on your wrist
    for 2 beats (desktop: hold Z for HP, X for TP, C for the menu); without a menu, your turn
    waits for it. Until then you take 20% less damage, your SOUL's glow shows your HP
    (bright = healthy, dim and flickering = low) and you feel a heartbeat paced by it.
    Meanwhile the earlier attacks cycle in 2-bar fragments.
  - **THE BLANK** (its last attack: the 8 bars before the FINAL HIT): the world turns to
    white line art on black, the song slows a little, it types `WH_T _S _ N_M_?` into its
    face, and its bullets are its unfinished letters drifting at you. You can't fall here.
- **The last turn**: the song holds its breath (the outro's rumble) while it waits. FIGHT is
  the finishing move: the song seeks one bar before the FINAL HIT, K1DD1E selects it, a DEL
  key floats in front of you, and the blow lands exactly on the hit (don't punch and he does
  it for you).
- **ACTs**: CHECK ("* _____ - ATK 22 DEF 10. It's what's left when something is deleted.
  It doesn't have a name. Nobody ever gave it one."), LISTEN (reads its broken sentences;
  each use fills one more letter back into every line), TALK, and CTRL+ALT+DEL. Items:
  CACHED COOKIE x2 (22 HP) and ★SAVE STATE x1 (both of you back to full). MERCY: there's
  nothing in its bar.
- **GAME OVER**: your SOUL is deleted, it turns into underscores and they fade; "NO. ctrl z.
  CTRL Z." Retry starts the phase over, both of you at full HP, TP at 50%.
- Debug: `\` in boss 2's fight plays the tape (on your turn), then each press takes
  MERCY down another 25; in the shop it puts a tape in your bag. `N` moves the secret fight
  on a phase like any other.
