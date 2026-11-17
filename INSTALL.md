# Seat Wheel - Setup Guide for Teachers

Seat Wheel spins a wheel of student names to assign seats at random. It works with no internet connection.

## Install

The files are in the `release` folder.

### Windows

Pick one:

- **Seat Wheel Setup (Windows).exe** installs the app and adds a "Seat Wheel" shortcut to the desktop and Start menu. It doesn't need an admin password.
- **Seat Wheel Portable (Windows).exe** doesn't install anything. Double-click it and it runs, so it works from a USB stick.

The first time it runs, Windows may say "Windows protected your PC." Click **More info**, then **Run anyway**. You only do this once.

### Mac

1. Check your Mac type: Apple menu > About This Mac. "Chip: Apple M..." means **Apple Silicon**. "Processor: Intel" means **Intel**.
2. Double-click the matching zip, then drag **Seat Wheel** into Applications.
3. Open Terminal (Applications > Utilities), paste this line, and press Return. You only do this once per Mac:

   ```bash
   xattr -cr "/Applications/Seat Wheel.app" && codesign --force --deep -s - "/Applications/Seat Wheel.app"
   ```

4. Open Seat Wheel. If the Mac says it can't verify the developer, go to System Settings > Privacy & Security and click **Open Anyway**.

## First-time setup

Settings opens by itself the first time.

1. **Classes:** click **Add a class**, name it (for example "Period 1"), and paste your class list with one name per line. A list copied from a spreadsheet column works. You can have up to 6 classes.
2. **Look:** you can change the title and subtitle (for example "Room 210" and your name), and pick three colors to match your school.
3. **Sounds:** turn the built-in sounds on or off, set the volume, or click **Add sound files** to use your own MP3, WAV, M4A or OGG clips. Short clips of 1 to 3 seconds work best.
4. Click **Done**.

Everything saves on that computer by itself: classes, colors, sounds, and the seat chart you're building. Close the app and it's all still there next time.

## Each class

- Click the class name at the top right.
- Open **Absent today** and uncheck anyone who is out.
- Press **SPIN** or the spacebar. Each name comes off the wheel once it's picked.
- **Fill all seats** finishes the rest at random. **Start over** clears the chart for that class.
- Full screen: the button, or F11 on Windows, or Control + Command + F on a Mac.

## Moving or sharing your setup

In Settings, click **Save backup file**. This saves one file with your classes, colors and sounds.

- On a new computer, install Seat Wheel, go to Settings, and click **Open backup file**.
- To share your colors and sounds with another teacher, send them the backup file. They can then replace the class lists with their own. Your backup contains your student names, so only share it inside your school.

## Updating the app (for whoever maintains it)

Requires Node.js. In a terminal in this folder:

```bash
npm install
```

```bash
npm start
```

```bash
npm run build:win
```

```bash
npm run build:mac
```

New files go in the `dist` folder. Teachers' saved classes and sounds are kept when they install a new version.
