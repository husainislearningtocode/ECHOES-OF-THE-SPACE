INTRO (intro.js + intro.css)
FILES   intro.js, intro.css next to index.html. Optional: assets/intro.json, assets/logos/*.png, assets/audio/intro.mp3
LOGOS   put assets/logos/nasa-space-apps.png and assets/logos/bahrain-space-agency.png. Missing file = the name is shown as text.
        Paths (local or URL, "" = skip) can be changed in assets/intro.json. The logo card is light so dark logos stay readable.
        (edit .iCard background in intro.css if your logos are white.)
FLOW    logos+title on black -> story lines on black -> fade into 3D planets -> machines one by one -> interactive scene.
TIMINGS Top of intro.js, CFG.beats: min = minimum seconds, out = fade-out seconds, mid = machine id (to add/remove/reorder machines in the tour).
        A beat ends only when its min time AND its narration line are both done. Total about 65-75 s.
TITLE   CFG.title (top of intro.js). Story lines: the 'kind: story' beats. Machine lines are built from missions data (name, date, status).
NARRATION  CFG.beats[].text is both the spoken line and the caption. Edit it there. Voice: CFG.voice (rate .92, pitch .9).
RECORDED VOICE  Put an mp3 at assets/audio/intro.mp3 and set in assets/intro.json:
        "audio": "assets/audio/intro.mp3", "cues": [...]
        cues = start second of each beat, in order (14 numbers: logos, s1-s4, reveal, 7 machines, outro). If the file is missing or blocked,
        the browser voice is used silently. (assets/intro.json is loaded with fetch(), so use a local server.)
SKIP    Skip button or Esc. The choice is remembered (localStorage key eos_skip_intro). "Replay intro" button appears top right.

----- TWO VERSIONS -----
FULL     CFG.beats in intro.js (the flow above). The logo screen no longer has a spoken line: it is VFX only (rings, scan line, sparks, title glitch, light sweep).
         If you use a recorded voice, the first cue now covers a silent beat of about 3.4 s.
SHORT    CFG.short in intro.js: logos VFX -> planets -> 3 machines -> outro, about 12 s, captions only, no narration, faster gate hand-over.
         Change machines or timings in that list (min = seconds).
START    The opening screen shows "Full intro" and "Quick intro". Top right (after the intro): "Replay intro" and "Quick intro".
LOGOS    The strip is a white band 100% of the screen width and 10% of its height (height: clamp(64px,10vh,140px) on .iCard in intro.css).

----- VERSION 3 -----
LOGO STRIP  Pinned to the top edge of the screen. Each logo is scaled to fit inside the white band (max-height 100% of the band, with padding).
QUICK INTRO Now about 15 s: the black logo screen (1.5 s) -> planets (1.1 s) -> every machine that loaded, oldest first (1967 to now, 0.8 s each, whip camera,
            flash, speed lines, sparks, dust, FOV punch, swoosh + impact sounds) -> year counter runs on to today -> hand-over.
            The machine list is built from the machines that really loaded (buildShort() in intro.js), so a missing model can never cut it short.
            Tweak: per-machine time (min: .8) and the logo / reveal / outro times in buildShort().
EFFECTS     Particles (sparks, dust, floating motes) and a glowing atmosphere rim on Earth and Mars (atmos() in intro.js). Sounds: swoosh, impact and a thin Mars wind (object A).
STORIES     Astronaut figures removed. Added dust, sparks, engine exhaust, floating motes, Mars dust haze / moon fog, atmosphere glow on planets, and sound effects
            (wind, engine rumble, impact, swoosh). Apollo 11 "The Legacy" now stays close to the descent stage (camera distance 10 to 30).

----- VERSION 4 -----
PARALLAX SPACE  The opening screen (logos, title, intro buttons) now sits on a parallax star field: 4 depth layers, soft red/blue nebula glows, a shooting star every few seconds.
                It follows the pointer (and tilt on phones); when idle it sways on its own. It fades out with the black layer, then stops drawing. Reduced-motion = one still frame.
                Code: SPACE in intro.js (star counts / sizes / nebula colours in build()). Style: #iSpace and #iBlack::after in intro.css.
QUICK INTRO     About 35 s and unhurried. The year counter starts at CFG.quickStart (1970, top of intro.js). Machines from that year or earlier (Surveyor 3, Apollo 11, ALSEP) are already awake
                in the wide shot ("By 1970, three machines were already out there."). Then the camera visits each later machine that loaded, oldest first (3.4 s each, same slow camera fly as the full intro).
                While the camera rests on a machine, the counter climbs to the NEXT machine's year, so a machine wakes exactly when the counter lands on its year (quickYear()). The counter ends on today's year.
                The fast whip camera, flash and speed-line punches were removed from this version. Tweak: min values in buildShort(). Set quickStart to 1960 to include every machine in the tour.
