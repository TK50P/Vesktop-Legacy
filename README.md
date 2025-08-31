# Vesktop

Vesktop is a custom Discord desktop app

> [!NOTE]  
> If you're looking for ported version of Equicord's Equibop, please look [here](https://github.com/TK50P/Equibop-Legacy).

**Main features**:
- Vencord preinstalled
- Much more lightweight and faster than the official Discord app
- Windows NT 6.x (Vista _with [Extended kernel](https://win32subsystem.live/extended-kernel/download/)_, 7, 8, 8.1) Support
- Windows 32-bit support
- Much better privacy, since Discord has no access to your system

**Not yet supported**:
- Global Keybinds
  
<img width="1920" height="1080" alt="Screenshot 2025-08-30 144257" src="https://github.com/user-attachments/assets/d323e20d-afe8-472f-92ba-da6b8b7b9f1a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5605d220-3cf6-41b2-ae8c-5f36443eb9ce" />
<img width="1920" height="1080" alt="Screen_Shot_2025-08-31_at_5 17 42_PM" src="https://github.com/user-attachments/assets/4e2b4f20-7792-4c47-a981-9513742e9663" />

## Installing

Visit https://vesktop.vencord.dev/install

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
- [Modified Electron 28](https://github.com/TK50P/electron-port/releases/download/v28.0.0/local_electron.zip)  
- [electron.js](https://github.com/TK50P/electron-port/releases/download/v28.0.0/electron.js)

Place the unpacked, modified version of Electron 28 in the root of the source and name the folder `local_electron`.  
Inside this folder, you **must** include the files:  
- `electron-v28.0.0-win32-x64`
- `electron-v28.0.0-win32-ia32`  
- `package.json`

Place the `electron.js` in `scripts` folder.

Now open `package.json`. Replace `pnpm build && electron .` with `node scripts/electron.js .`. <br>
In `"devDependencies"` section, replace `"electron"`'s version (e.g. `"^37.2.0"` with `"file:./local_electron"`). 

Now, go to `"build"` section and add this line.
```js
"electronDist": "./local_electron/electron-v28.0.0-win32-x64",
"electronVersion": "28.0.0",
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
        "electronDist": "./local_electron/electron-v28.0.0-win32-x64",
        "electronVersion": "28.0.0",
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
git clone https://github.com/Equicord/Equibop
cd Equibop

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
For macOS, the setup is simpler than on Windows.
> [!IMPORTANT]  
> You **must** be using macOS Catalina as **host** to build it.

You can simply downgrade the Electron version as follows:

```sh
git clone https://github.com/Equicord/Equibop
cd Equibop

# Install dependencies
pnpm i

# Downgrade Electron to v32 (last version supported on Catalina, based on Chromium 128)
pnpm i -f electron@32

# Package the app
pnpm package
```
