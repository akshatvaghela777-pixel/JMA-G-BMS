# Building the Windows EXE

The Lovable sandbox cannot produce Windows binaries. Run these on any Windows
machine with Node.js 18+ installed.

```cmd
npm install
npm run package:win   :: portable EXE  -> release-electron\JMA-G-BMS-Portable.exe
npm run dist:win      :: installer EXE -> release-electron\JMA-G-BMS-Setup.exe
```

Dev preview (web): `npm run dev`
Electron dev shell:  `npm run electron:dev`
