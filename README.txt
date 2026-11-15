ECHOES OF SPACE

RUN: python3 -m http.server 8000 in this folder, then open http://localhost:8000
     (double-clicking index.html will not work: the mission files are loaded with fetch()).

FILES
  index.html, style.css, app.js (3D scene, timeline, panel), story.js (animated stories + panel preview)
  assets/missions/index.json   list of mission ids
  assets/missions/<id>.json    one file per mission (text, year, status, fallback shape)
  assets/models/<id>.glb       3D model for that mission (see ROVERS below for the rule)
  assets/models/<planet>.glb   3D model for a planet (see PLANET MODELS below)
  gate.js, gate.css            sci-fi blast-gate loading screen (see GATE below)
  assets/textures/moon.jpg, mars.jpg, earth.jpg   NASA maps (optional)

ADD A MISSION: copy one file in assets/missions, change "id", set "body" to moon, mars or space, add the id to index.json.
STATUS values: parked, silent, grounded, active.
Unique animated stories so far: Sojourner, Apollo 11, Dawn, Kepler, NEAR Shoemaker, Pioneer 10. Other Moon/Mars missions use a shared template; other deep-space missions show the panel only.
IMPORTANT: keep the assets/ folder next to index.html or no machines will load. Use the zip.
CONTROLS: drag = look, wheel/pinch = zoom, arrows = orbit, + / - = zoom, Esc = back.
CREDITS: mission facts from NASA. Add the credit line for each NASA model and texture you use.

LOOK: black surfaces, white text, NASA-logo red (#fc3d21) as the only accent. Colours are the variables at the top of style.css (--red, --void, --text).
FONT: Montserrat (Google Fonts link in index.html). Offline use: put assets/fonts/Montserrat-Variable.woff2 (or .ttf) in place.

PLANET MODELS (any .glb works; no code change)
  Put a file named after the body in assets/models/ and reload:
    mars.glb  moon.glb  earth.glb  mercury.glb  venus.glb  jupiter.glb  saturn.glb  uranus.glb  neptune.glb  sun.glb
  The model is centred and sized to the planet's diameter. A missing file keeps the low-poly planet. The gate waits for the files.
  assets/planets.json: optional per-planet "scale" (1.2 = 20% bigger), "rotation" (degrees [x,y,z]) and "model" (another path).
  Saturn: if your model includes the rings, use scale around 0.6. The Sun model is shown unlit (it glows by itself).
  Draco-compressed .glb files work too (DRACOLoader is included in index.html).

ROVERS (the "only load the ones that have a model" rule)
  Every rover has a mission file (Sojourner, Spirit, Opportunity, Curiosity, Perseverance, Lunar Roving Vehicle).
  A rover only appears (timeline, fleet list, scene) when the .glb named in its "model" field exists. No file = not loaded, no stand-in shape.
  The browser console lists the rovers it skipped. To add one: drop the file named in assets/missions/<id>.json ("model") and reload.
  Landers, orbiters and probes are not affected: they still show a stand-in shape until their model exists.
  Spirit uses its own file, assets/models/spirit.glb (see "model" in its mission file). If you only have opportunity.glb (same design), point Spirit's "model" at it.

GATE (loading doors)
  Sealed at page load. The ring shows real loading progress of the missions and every .glb. Then the bolts retract, the doors part, screen shake.
  It closes and opens again when the intro ends or is skipped. It cannot trap anyone: it opens after 60 s even if a file never arrives.
  Settings at the top of gate.js (CFG). Doors are plain HTML in index.html (#gate); look is in gate.css. Sound plays only if the browser allows it.

SCREEN SHAKE
  Clicking a planet or machine, the camera landing on it, zooming close to it, and leaving it each shake the camera. EOS.shake(0..1) in app.js.
  Switched off when the visitor's system asks for reduced motion.

----- ADDED IN THIS VERSION -----
GALLERY (assets/gallery.json)
  Images for each machine appear in a "Gallery" section of its panel; click one to enlarge (arrows / Esc work).
  assets/gallery.json:  { "base": "assets/images/", "missions": { "<mission id>": [ ... ] } }
  Each entry is a URL ("https://..."), a file name inside "base" ("sojourner-1.jpg"), or an object:
    { "type": "url" | "file", "src": "...", "caption": "...", "credit": "NASA/JPL" }   ("type" is optional: http/https/data: = URL, anything else = file)
  Put your own files in assets/images/. A broken or missing image is skipped; with no images the section stays hidden.

INTRO VERSIONS
  The opening screen has two buttons: "Full intro" (the long one) and "Quick intro" (about 35 s, the year counter starts at 1970 and runs slowly to today, captions only). Top right: "Replay intro" and "Quick intro".
  See README-intro.txt. Console: EOS.playIntro('short') or EOS.playIntro('long').

STORY PLAYER
  The chapter description sits at the top left of the animation; the Close button is at the top right.

MODELS FROM assets/
  Planets, moons and asteroids use assets/models/<name>.glb in the main scene AND inside every mission story:
    mars moon earth mercury venus jupiter saturn uranus neptune sun   (scene)      vesta ceres eros   (story scenes: Dawn, NEAR Shoemaker)
  Spacecraft in stories use the .glb named in their mission file (Dawn, Kepler, NEAR Shoemaker, Pioneer 10 as well as the rovers and landers).
  A low-poly stand-in is used only for a file that is missing. Press F12: the console lists every model file it could not find.
