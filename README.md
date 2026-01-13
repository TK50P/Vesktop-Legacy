# Vesktop-Legacy

Vesktop is a custom Discord desktop app but for Legacy OSes support

> [!CAUTION]
> Usage of unofficial or ported clients is not guaranteed to function as intended. Future updates may break certain features or cause unexpected behavior.
>
> **Don’t be stupid** — do **not** open issues or contact Vencord support. These clients are not supported or affiliated with **the official Vesktop** in any way.
>
> **Use at your own risk.**

**Main features**:
- Vencord preinstalled
- Much more lightweight and faster than the official Discord app
- Windows NT 6.x (Vista _with [Extended kernel](https://win32subsystem.live/extended-kernel/download/)_, 7, 8, 8.1) Support
- Windows 32-bit support
- macOS Catalina 10.15 Support
- Much better privacy, since Discord has no access to your system

**Not yet supported**:
- Global Keybinds

<img width="1920" height="1080" alt="Windows Vista-2025-11-22-12-03-07" src="https://github.com/user-attachments/assets/255ca533-e0fa-4835-aa08-fb3890cc4f2c" />
<img width="1920" height="1080" alt="Screenshot 2025-08-30 144257" src="https://github.com/user-attachments/assets/a23d305b-774b-44b7-a0b5-47eb20b4ef72" />
<img width="1920" height="1080" alt="Windows_8_x64-2026-01-13-16-43-29" src="https://github.com/user-attachments/assets/f08eaf23-1da4-4239-9ae8-3ba0df460b36" />
<img width="1920" height="1080" alt="Windows 8 1-2025-11-02-22-18-22" src="https://github.com/user-attachments/assets/0fe27733-68a8-43d6-8d44-8e84bf1d0db7" />
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
You’ll need the following this file:  
- [Modified Electron](https://github.com/e3kskoy7wqk/Electron-for-windows-7) (Thanks to [@e3kskoy7wqk](https://github.com/e3kskoy7wqk))

Place the unpacked `dist-(x86).zip` in `local_electron`, rename to `electron-v37.2.2-win32-x64` for 64-Bit, and `electron-v37.2.2-win32-ia32` for 32-Bit.

Inside this folder, you **must** include the files:  
- `electron-v37.2.2-win32-x64` (for 64-Bit)
- `electron-v37.2.2-win32-ia32` (for 32-Bit)

Now open `package.json`. Replace `pnpm build && electron .` with `pnpm build && local_electron\\electron-v37.2.2-win32-x64\\electron.exe .`. <br>
In `"devDependencies"` section, replace `"electron"`'s version (e.g. `"^37.2.2"` with `"file:./local_electron"`). 

Now, go to `"build"` section and add this line.
```js
"electronDist": "./local_electron/electron-v37.2.2-win32-x64",
"electronVersion": "37.2.2",
```
> [!NOTE]
> You must change `x64` to `ia32` if you are targetting to 32Bit.

For Example, if code is like this,
```js
    "build": {
        "appId": "dev.vencord.vesktop",
        "productName": "Vesktop",
        "executableName": "vesktop",
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
        "productName": "Vesktop",
        "executableName": "vesktop",
        "electronDist": "./local_electron/electron-v37.2.2-win32-x64",
        "electronVersion": "37.2.2",
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
