# 🌍🌙 Earth & Moon Orbital Simulator

An interactive 3D simulation of the **Sun–Earth–Moon system**, built to make the
geometry behind everyday astronomy *visible* and *intuitive*:

- Why the Moon has a **"dark side"** (spoiler: it doesn't — it has a *far* side)
- How **moon phases** arise from the Sun–Earth–Moon angle
- Why **solar and lunar eclipses** happen, and why they're **rare** instead of monthly
- How Earth's **axial tilt** produces the **seasons**

It runs entirely in the browser with [three.js](https://threejs.org/). There is
**no build step and no external image assets** — planet textures are generated
procedurally in code, so the only thing it fetches is the three.js library itself.

---

## ▶️ Running it

**Easiest:** just open `index.html` in a modern browser (Chrome, Firefox, Edge,
Safari). It loads three.js from a CDN, so you need to be online the first time.

**If you prefer a local server** (recommended, avoids any browser file-access
quirks):

```bash
# from this folder
python3 -m http.server 8000
# then open http://localhost:8000
```

---

## 🎮 Controls

| Control | What it does |
|---|---|
| **Pause / Play / Reset** | Stop, start, or return to day 0 |
| **Time speed** | Days per real second. Drag *left of center* to run time **backward**. |
| **−1 / +1 day, +1 month** | Step time precisely |
| **Camera views** | Jump between vantage points (see below) |
| **Show** toggles | Orbit paths, labels, the Moon's tilted orbital plane, the line of nodes & ecliptic, Earth's axis, eclipse shadows, sun rays, starfield |
| **Jump to next…** | Fast-forward to the next solar eclipse, lunar eclipse, full moon, or new moon |
| **Guided demos** (bottom bar) | One-click tours of each concept, with on-screen explanations |
| Mouse | Drag = orbit · scroll = zoom · right-drag = pan (in *Free orbit* view) |
| Keyboard | `Space` = play/pause · `.` = +1 day · `,` = −1 day |

### Camera views
- **Free orbit** — fly around manually.
- **Earth–Moon top** — looks straight down on the Earth–Moon system.
- **Earth–Moon edge** — looks along the line of nodes so the Moon's 5.14° tilt is visible edge-on (great for understanding eclipses).
- **Tidal-lock cam** — frames Earth and Moon together; the green near-side marker stays glued toward Earth.
- **From Earth → Moon** — a telescope-style view *from* Earth, so you literally see the current phase.
- **Heliocentric top** — zooms out to the whole orbit of Earth around the Sun.

---

## 🔭 The science, and the honest fudge on scale

Real space is mostly empty, so a literally-to-scale model would show three
invisible specks. This simulator keeps the quantities that **teach** the
phenomena exactly right, and compresses only the one that would otherwise make
everything invisible.

**1 scene unit = 1 Earth radius.**

| Quantity | In the sim | Real value | Faithful? |
|---|---|---|---|
| Earth radius | 1 | 6,371 km | ✅ reference |
| Moon radius | 0.273 | 1,737 km | ✅ real ratio |
| Earth–Moon distance | 60.3 | ~60.3 Earth radii (384,400 km) | ✅ real |
| Moon orbit inclination | 5.14° | 5.14° to the ecliptic | ✅ real |
| Earth axial tilt | 23.44° | 23.44°, fixed in space | ✅ real |
| Tidal locking | exact | exact (synchronous rotation) | ✅ real |
| Sidereal month | 27.32 d | 27.32 d | ✅ real |
| Sidereal year | 365.26 d | 365.26 d | ✅ real |
| Earth–Sun distance | 1,500 | 23,481 Earth radii (1 AU) | ⚠️ compressed ~15× |
| Sun radius | angular-matched | 109 Earth radii | ⚠️ see below |

**Why the Sun is "wrong" on purpose.** The Sun is drawn ~15× closer than reality
so the whole system fits on screen. To keep **eclipses** correct, the Sun's
radius is then chosen so its **angular size as seen from Earth (~0.53°) nearly
equals the Moon's** — which is the real cosmic coincidence that makes total solar
eclipses possible. So the Sun's *physical* size isn't to scale, but its *angular*
size — the only thing that governs eclipse geometry — is.

### What's actually being simulated
- The **Moon's orbital plane is tilted 5.14°** and its **line of nodes is fixed**
  along one axis. Eclipses can only happen when the Sun lines up with that node
  line — which occurs in two "eclipse seasons" roughly half a year apart, instead
  of every month. This is *the* reason eclipses are rare.
- **Eclipses are detected geometrically** by comparing the angular separation of
  the Sun and Moon (or Moon and Earth's shadow) against their angular radii. The
  3D shadows you see are real shadow-mapped shadows: the Moon's shadow falling on
  Earth (solar eclipse) and Earth's shadow falling on the Moon (lunar eclipse).
- **Tidal locking** is enforced exactly — the Moon turns once per orbit so the
  same hemisphere always faces Earth. Watch sunlight sweep across the far side to
  see why "dark side of the Moon" is a misnomer.
- **Earth's tilt stays fixed in space** as it orbits (axial parallelism), so the
  hemisphere leaning sunward changes through the year — that's the seasons.

The orbital model has been numerically checked: it reproduces a **29.5-day
synodic month**, eclipse seasons **~177 days apart**, and roughly **two solar and
two lunar eclipses per year**, matching reality.

### Simplifications (kept deliberately simple for clarity)
- Orbits are treated as **circular** (real eccentricities are small: Earth ~0.017,
  Moon ~0.055), so the Moon distance readout is shown as a constant average.
- The Moon's **nodes don't precess** (real period 18.6 yr) and apsides don't
  rotate, so eclipse seasons here repeat on a clean ~half-year cadence rather than
  slowly drifting.
- Lighting uses a single directional light for the Sun's near-parallel rays at the
  Earth–Moon system.

---

## 🛠️ Tech
- [three.js](https://threejs.org/) r160 (ES modules via CDN import map)
- Procedural canvas textures for Earth, Moon, Sun glow, and stars — zero asset files
- Single self-contained `index.html`

## 🚀 Ideas for extending it
- Add elliptical orbits to show supermoons and annular vs total eclipses
- Add nodal precession to drift the eclipse seasons (Saros cycle)
- Overlay real ephemeris data (e.g. NASA JPL Horizons) for date-accurate events
- Add the Moon's libration

---

*Built as an educational toy — accurate where it teaches, simplified where it
clarifies.*
