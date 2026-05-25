# Battle Board — To Do

## Tasks
- [x] Push to GitHub
- [x] Set up hosting (Vercel)
- [ ] Buy a domain name
- [ ] Make app installable on phone (Progressive Web App / PWA)
- [ ] Export board layout as a shareable image
- [ ] Print-friendly mission card view

## Mobile (iOS & Android)

**Recommended path: Capacitor** — wraps the existing web app in a native shell.
The UI layer needs zero changes. Only storage and device APIs need swapping.

Steps when ready:
1. Set up a Vite build (converts the single HTML file into a proper build pipeline)
2. `npm install @capacitor/core @capacitor/cli @capacitor/preferences`
3. `npx cap init`
4. In `index.html`, swap `Storage.get/set` bodies for the Capacitor Preferences plugin
   (the `// MOBILE:` comments in the code mark exactly where)
5. `npx cap add ios` → open in Xcode → submit to App Store
6. `npx cap add android` → open in Android Studio → submit to Play Store

Device features that will need Capacitor plugins when built:
- [ ] Photo upload → `@capacitor/camera`
- [ ] Share board layout → `@capacitor/share`
- [ ] Push notifications (for campaign reminders) → `@capacitor/push-notifications`
- [ ] Local file export → `@capacitor/filesystem`

**Alternative: React Native** — heavier lift. The business logic (DataService, mission
data, assignment algorithm) ports cleanly. The render/UI layer (all the HTML template
functions) would need rewriting as React Native components. Worth considering if you
ever want deep native UI feel rather than a web wrapper.

## Code Structure (when splitting the single file)

When the project grows past one file, split like this:

```
src/
  data/
    missions.js        ← MISSIONS array + DEPLOYMENTS
    terrainTypes.js    ← TERRAIN_TYPES, T_VIS, SIZE_MULT, POS_MAP
  services/
    storage.js         ← Storage + DataService (the mobile-swap point)
    assignments.js     ← computeAssignments()
  ui/
    mapRenderer.js     ← deploymentSVG(), terrainSVGShape()
    vault.js           ← renderVault()
    missions.js        ← renderMissions()
    randomizer.js      ← renderRandomizer()
  app.js               ← state, render(), event handlers
index.html             ← shell only, imports app.js
```

## Potential Features

### Accounts & Data
- [ ] User accounts
- [ ] Save inventory to cloud (currently only saves in the browser you're on)
- [ ] Win / loss / draw record per army and per mission
- [ ] Campaign journal — log games with date, opponent, result, notes
- [ ] Upload pictures of maps when playing
- [ ] Save named board setups (e.g. "City Fight", "Open Desert") to reuse

### Game Tools
- [ ] VP tracker — live scoresheet during a game, turn by turn, both players
- [ ] Secondary objective picker / tracker (pick 3, score each round)
- [ ] Turn timer with optional clock-chess style per-player time
- [ ] Command point tracker
- [ ] Pre-built terrain layout templates from GW publications
- [ ] Table size selector (full 60×44", half-table, 44×44" for smaller games)

### Missions
- [ ] Custom missions — expand the current basic version with full slot editor
- [ ] Mission card deck mode — simulate drawing from a physical deck without repeats
- [ ] Community mission sharing — publish and browse missions others have made
- [ ] Narrative / Crusade mission type with special rules field
- [ ] Mission rule quick-reference panel (in-game reminders for that mission's special rules)

### Terrain
- [ ] Terrain database with official GW kit names and standard sizes pre-filled
- [ ] Terrain tags (e.g. "obscuring", "difficult ground", "scaleable") with rules tooltip
- [ ] Upload a photo of your terrain piece so it shows on the map instead of a shape
- [ ] Terrain set grouping (e.g. "Sector Mechanicus box", "Sector Imperialis box")

### Social / Club
- [ ] Share a mission link with your opponent so you're both looking at the same layout
- [ ] Club / league management — track standings across a group of players
- [ ] Tournament bracket tool
- [ ] Opponent history and notes

### Technical
- [ ] Set up Vite build pipeline (needed before Capacitor mobile build)
- [ ] Mobile layout improvements (the map is tight on small screens)
- [ ] Offline support — works fully with no internet after first load
- [ ] Dark / light mode toggle
- [ ] Keyboard accessibility
- [ ] Stats dashboard — charts of your most-played missions, terrain usage, win rates
