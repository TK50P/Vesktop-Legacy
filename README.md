# Vesktop-Legacy

Vesktop is a custom Discord desktop app but for Legacy OSes support

> [!NOTE]  
> If you're looking for ported version of Equicord's Equibop, please look [here](https://github.com/TK50P/Equibop-Legacy).

### Why I did this shit

I was curious if Vesktop would work on Windows 7. Tried it — **boom**, `DiscardVirtualMemory` error.  
So I opened an issue: [#363](https://github.com/Vencord/Vesktop/issues/363) — and the response?  
> "Upgrade to Windows 10."

**Seriously? That's it?** No fallback, no real reasoning — just brushed off.  
So I said screw it — I ported it myself.  
Now it runs on:
- Windows 7
- Windows 8 / 8.1
- **Even 32-bit systems**

Even worse? the NSIS Installer did not had OS detections and it just installed without ANY errors. more like **clickbait**.<br>
That’s the story. That’s why this exists.

**I am pissed off to Vencord devs.**
<!-- If you want this repository taken down, good luck. Maybe you shouldn't released this ever. -->

**Main features**:
- Vencord preinstalled
- Much more lightweight and faster than the official Discord app
- Windows NT 6.x (Vista _with [Extended kernel](https://win32subsystem.live/extended-kernel/download/)_, 7, 8, 8.1) Support
- Windows 32-bit support
- macOS Catalina 10.15 Support
- Much better privacy, since Discord has no access to your system

**Not yet supported**:
- Global Keybinds

<img width="1920" height="1080" alt="Screenshot 2025-08-30 144257" src="https://github.com/user-attachments/assets/a23d305b-774b-44b7-a0b5-47eb20b4ef72" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fa321b41-6ede-474a-9fc9-8624d8565373" />
<img width="1920" height="1080" alt="Screen Shot 2025-08-31 at 5 17 42 PM" src="https://github.com/user-attachments/assets/4c8eb491-1287-471d-bb42-5b73f1e0d6ff" />

## Building from Source

You need to have the following dependencies installed:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/en/download)
- pnpm: `npm install --global pnpm`

Packaging will create builds in the dist/ folder

> [!NOTE]
> On Windows, if you run the test script, you will get test errors about venmic, you can ignore these as it's a linux only module.

## For Windows
You’ll need the following 2 files:  
- [Modified Electron](https://github.com/e3kskoy7wqk/Electron-for-windows-7) (Thanks to [@e3kskoy7wqk](https://github.com/e3kskoy7wqk))
- [electron.js](https://raw.githubusercontent.com/TK50P/Equibop-Legacy/refs/heads/main/scripts/electron.js) and [package.json](https://raw.githubusercontent.com/TK50P/Equibop-Legacy/refs/heads/main/local_electron/package.json) (Use `curl` or `wget` to fetch this file)

Place the unpacked `dist-(x86).zip` in `local_electron`, rename to `electron-v37.2.2-win32-x64` for 64-Bit, and `electron-v37.2.2-win32-ia32` for 32-Bit.

Inside this folder, you **must** include the files:  
- `electron-v37.2.2-win32-x64` (for 64-Bit)
- `electron-v37.2.2-win32-ia32` (for 32-Bit)
- `package.json`

Place the `electron.js` in `scripts` folder.

Now open `package.json`. Replace `pnpm build && electron .` with `node scripts/electron.js .`. <br>
In `"devDependencies"` section, replace `"electron"`'s version (e.g. `"^37.2.2"` with `"file:./local_electron"`). 

Now, go to `"build"` section and add this line.
```js
"electronDist": "./local_electron/electron-v37.2.2-win32-x64",
"electronVersion": "37.2.2",
```

For Example, if code is like this,
```js
    "build": {
        "appId": "dev.vencord.vesktop",
        "productName": "Vesktop",
        "files": [
            "!*",
            "!node_modules",
            "dist/js",
            "static",
            "package.json",
            "LICENSE"
        ],
```

Place like this.
```js
    "build": {
        "appId": "dev.vencord.vesktop",
        "electronDist": "./local_electron/electron-v37.2.2-win32-x64",
        "electronVersion": "37.2.2",
        "productName": "Vesktop",
        "files": [
            "!*",
            "!node_modules",
            "dist/js",
            "static",
            "package.json",
            "LICENSE"
        ],
```

### How to Build 32-Bit Version of Vesktop

To build a **32-bit** version of Vesktop, change all occurrences of `x64` to `ia32`.

In the following section, remove any other architecture definitions and ensure both the `nsis` and `zip` targets are explicitly set to a specific architecture (`x64` or `ia32`):

```js
"win": {
  "target": [
    {
      "target": "nsis",
      "arch": [
        "ia32"
      ]
    },
    {
      "target": "zip",
      "arch": [
        "ia32"
      ]
    }
  ]
},
```

Now, run this.

```sh
# Set architecture FIRST to build 32-bit (ia32) target
set npm_config_arch=ia32

git clone https://github.com/Vencord/Vesktop
cd Vesktop

# Install Dependencies
pnpm i

# Compile TypeScript files
pnpm build

# Now, start the program
pnpm start

# Or package it for Windows
pnpm package
```

## For macOS Catalina (10.15)  
> [!NOTE]  
> Since macOS Catalina only supports Intel Macs, so building with universal binary is pointless.
> 
> You can replace `universal` with `x64` to build Intel Macs (x64 binaries) only.

Open `package.json` and replace like this.
```js
        "mac": {
            "target": [
                {
                    "target": "default",
                    "arch": [
                        "universal"
                    ]
                }
            ],
            "category": "public.app-category.social-networking",
            "darkModeSupport": true,
            "extendInfo": {
                "NSMicrophoneUsageDescription": "This app needs access to the microphone",
                "NSCameraUsageDescription": "This app needs access to the camera",
                "com.apple.security.device.audio-input": true,
                "com.apple.security.device.camera": true,
                "CFBundleIconName": "Icon"
            },
            "notarize": true
        },
```
Change to like this.
```js
        "mac": {
            "target": [
                {
                    "target": "default",
                    "arch": [
                        "x64"
                    ]
                }
            ],
            "minimumSystemVersion": "10.15.0",
            "category": "public.app-category.social-networking",
            "darkModeSupport": true,
            "extendInfo": {
                "NSMicrophoneUsageDescription": "This app needs access to the microphone",
                "NSCameraUsageDescription": "This app needs access to the camera",
                "com.apple.security.device.audio-input": true,
                "com.apple.security.device.camera": true,
                "CFBundleIconName": "Icon"
            },
            "notarize": true
        },
```

Now, simply downgrade the Electron version as follows:

```sh
git clone https://github.com/Vencord/Vesktop
cd Vesktop

# Install dependencies
pnpm i

# Downgrade Electron to v32 (last version supported on Catalina, based on Chromium 128)
pnpm i -f electron@32

# Package the app
pnpm package
```
