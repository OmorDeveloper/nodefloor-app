# Install and first run

## Windows

1. Download **`NodeFloor-<version>-win-x64-setup.exe`** from the
   [latest release](https://github.com/OmorDeveloper/nodefloor-app/releases/latest).
2. Run it. **Windows SmartScreen will warn you** — the installer is not
   code-signed. Click *More info*, then *Run anyway*.
3. Launch NodeFloor. It asks you to pick a **home folder** on first run.

Prefer not to install? `NodeFloor-<version>-win-x64-portable.exe` is the same
app in a single file. It runs from anywhere, including a USB stick, and writes
its home folder wherever you point it.

### About that SmartScreen warning

It is not a false positive to shrug off — it means exactly what it says, which
is that Windows has never seen this publisher sign anything. A signing
certificate is a real cost the project has not paid yet. Until it does, the
honest position is: the warning is correct, and you are trusting the download
link and the checksum rather than a certificate.

Every release lists SHA-256 checksums. On Windows:

```powershell
Get-FileHash .\NodeFloor-0.4.5-win-x64-setup.exe -Algorithm SHA256
```

## macOS and Linux

Builds are produced for both and get far less testing than Windows — treat them
as usable rather than supported.

- **macOS** — `.dmg`. Not notarised, so Gatekeeper blocks the first launch:
  right-click the app → *Open* → *Open*. Or
  `xattr -dr com.apple.quarantine /Applications/NodeFloor.app`.
- **Linux** — `.AppImage`. `chmod +x` it and run it.

## What it needs

**A coding CLI you already have.** NodeFloor drives an agent CLI rather than
talking to a model itself, so it runs on whatever subscription you already pay
for. It works with Claude Code, Codex, Gemini CLI and several others; it finds
the ones on your `PATH` and tells you which it found.

**Nothing else.** Hermes ships inside the app with its own Python. There is no
runtime to install, no database to set up, and no account to create.

## The home folder

On first run you choose a **NodeFloor home** — one folder holding everything for
one workspace: settings, your nodes and their memory, tasks, triggers, and
history.

Each home is separate and self-contained, so you can keep several and switch
between them: one per client, or one for work and one for experiments. The
picker on launch lets you open the last one, open another, or start a new one.

Back it up by copying the folder. There is nothing else to export.

## Updating

The app checks for new releases on launch and tells you when one is out. It
never updates itself behind your back.

## Uninstalling

Windows: *Add or remove programs* → NodeFloor. Your home folder is **left
alone** — uninstalling the app must not delete your work. Delete that folder
yourself if you want it gone.
