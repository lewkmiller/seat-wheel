# Seat Wheel

A spinning name wheel for assigning classroom seats at random, shown on a projector. Mr. Miller (Nampa Christian Middle School art, Room 210) built it for himself, and as of v2.0 it is generic so any teacher can use it: no names or school branding are built in, and each teacher sets up their own classes, title and colors.

The user is a teacher, not a programmer, and works in VS Code. Keep explanations plain and steps short. No emojis.

## Files

- `app/index.html` is the whole app: HTML, CSS and JavaScript in one file. It still runs on its own by double-clicking it in Chrome or Edge.
- `app/fonts/` holds Bricolage Grotesque and Inter (latin subset, woff2) and `fonts.css`, so the app needs no internet. Google Fonts is no longer used.
- Desktop app: Electron wraps `app/index.html`. `main.js` opens the window (F11 toggles full screen, menu bar hidden on Windows). `package.json` holds the electron-builder config. `build/icon.png` is the app icon (1024px plain wheel in the default navy, gold and silver).
- Build: `npm install`, then `npm start` (test), `npm run build:win` (NSIS installer + portable exe, x64), `npm run build:mac` (zip, arm64 and x64, unsigned). Output goes to `dist/`. Building Windows on Linux or Mac needs wine.
- `release/` holds the current builds, renamed for the user. `INSTALL.md` is the install and setup guide for teachers.
- Builds are not code-signed. Windows shows SmartScreen ("More info" > "Run anyway"). Mac builds need a one-time `xattr -cr` + ad-hoc `codesign --force --deep -s -` on the Mac, or Apple Silicon reports the app as damaged.

## How it works

- All teacher data lives in one object, `data`, saved to localStorage under `seatwheel.v1` on every change: title, subtitle, 3 colors, sound settings, `current` class id, and `classes` (max 6, `MAX_CLASSES`). Each class is `{id, name, roster[], absent[], seats[]}`, so seat charts and absences survive a restart. In Electron, localStorage persists in the app's user-data folder.
- Settings is a `<dialog>`. It opens automatically when there are no classes. Rosters are pasted one name per line (commas and tabs also split). `parseRoster()` trims spaces and renames duplicates "Name (2)". Editing a roster drops seats and absences for removed names.
- Each spin picks a random name from the remaining pool, assigns the next seat number, and removes that name from the wheel.
- "Absent today" checkboxes take students off the wheel.
- Buttons: SPIN (spacebar also works), Fill all seats, Start over, Full screen (falls back to an F11 hint), Sound on/off, Settings.
- Spin lasts 2.4 seconds with 3 to 4 rotations (`dur` and `turns` in `spin()`).
- Sounds: all audio goes through a `master` gain node (volume slider). Optional click per slice. On landing, one random enabled sound plays, never the same one twice in a row, chosen from the 8 synthesized built-ins (`FUNNY`, labels in `BUILTIN_NAMES`) plus the teacher's own files. Custom files (any format the browser decodes, 10 MB max) are stored as ArrayBuffers in IndexedDB (`seatwheel` db, `sounds` store) because localStorage is too small. If IndexedDB fails they work only until the app closes.
- Backup: "Save backup file" downloads JSON `{app:"seat-wheel", version, settings, sounds:[{name,type,on,data(base64)}]}`. "Open backup file" replaces everything. "Erase everything" resets.
- Colors: the teacher picks 3. `applyColors()` builds the 6 wheel colors in `COLORS` (the 3 plus a darker color 1 and lighter colors 2 and 3), sets CSS variables `--c1/--c2/--c3`, and picks black or white text by luminance (`textOn`). Light mode uses color 1 as the accent, dark mode uses color 2. Defaults are navy, gold and silver (`DEFAULT_COLORS`).
- A small confetti burst plays from the pointer on landing.
- User-entered names are escaped with `esc()` before going into HTML.

## Open to-do

- Optional logo upload in Settings (store in IndexedDB like sounds) shown in the header and wheel hub.
- Code signing: Windows certificate and Apple Developer ID plus notarization would remove the SmartScreen and Terminal steps. This needs paid accounts.
- Maybe: CSV import, or more than 6 classes.
