# Seat Wheel

A spinning name wheel teachers use to assign classroom seats at random. It's made for a projector and works offline.

- Up to 6 classes. Paste each class list once and the app remembers it.
- "Absent today" checkboxes, Fill all seats, Start over, and Full screen.
- Pick your own title and three school colors.
- 8 built-in funny landing sounds, plus your own MP3, WAV, M4A or OGG files.
- Backup files to move a setup to another computer.

## Download

Get the Windows and Mac apps from the **Releases** page (right side of this repo page). See [INSTALL.md](INSTALL.md) for install and setup steps.

You can also open `app/index.html` directly in Chrome or Edge, with no install.

## Build it yourself

Requires Node.js.

```bash
npm install
npm start            # run it
npm run build:win    # Windows installer + portable exe
npm run build:mac    # Mac zips (Apple Silicon and Intel)
```

Built files go in `dist/`.
