# SteamClip

[![Total Downloads](https://img.shields.io/github/downloads/Nastas95/SteamClip/total?style=for-the-badge&color=orange)](https://github.com/Nastas95/SteamClip/releases)
[![Python Version](https://img.shields.io/badge/Python-3.8+-yellow?style=for-the-badge&logo=python)](https://python.org)
[![License](https://img.shields.io/github/license/Nastas95/SteamClip?style=for-the-badge&color=green)](LICENSE)
[![Platforms](https://img.shields.io/badge/Platforms-Windows%20%7C%20Linux-blue?style=for-the-badge&logo=platformdotsh)](https://github.com/Nastas95/SteamClip)

> A GUI converter for Steam game recordings

Steam stores recordings as segmented `.m4s` files (DASH format). The native export works, but often produces pixelation and stuttering. SteamClip converts those recordings to clean `.mp4` files using FFmpeg, with no artifacts and no length limits

---

## Features

+ **Clip browser** — your recordings are displayed in a thumbnail grid with page controls. Click one or more clips, hit convert, done. The output lands on your Desktop by default

+ **Share to YouTube** — select a single clip and click "Share to YouTube" to upload it directly. A dialog lets you set the title, description, and privacy level (Unlisted by default). See [YouTube Integration](#youtube-integration) for setup

+ **Automatic game names** — SteamClip identifies the game for each clip automatically, including non-Steam games added to your Steam library (emulators, EmuDeck, Epic, etc.). You can also assign custom names from the settings

+ **FFmpeg bundled** — no separate installation needed

+ **13 built-in themes** — Steam Dark, Cyberpunk, Neon Blue, Dracula, Nord, Gruvbox, Catppuccin, Pip-Boy, CRT Amber, and a few more. Follows your OS light/dark setting automatically if you prefer

+ **Privacy** — no data collection. Everything is stored locally. The only outbound connections are Steam API calls for game names and GitHub release checks for updates

---

## Installation

+ **Windows:** download `steamclip.exe` from the [Releases page](https://github.com/Nastas95/SteamClip/releases)

+ **Linux:** download the binary from the [Releases page](https://github.com/Nastas95/SteamClip/releases) 

+ **run from source:**

```bash
git clone https://github.com/Nastas95/SteamClip
cd SteamClip
pip install PyQt6 imageio[ffmpeg] pillow requests pathvalidate google-api-python-client google-auth-oauthlib google-auth-httplib2
python steamclip.py
```


On first launch, select your Steam installation type: Standard, Flatpak, or Manual (if your `userdata` folder is somewhere non-standard)

---

## Building from source

```bash
pip install pyinstaller PyQt6 imageio[ffmpeg] pillow requests pathvalidate
pyinstaller --onefile --windowed steamclip.py
```

---

## Data storage

- **Windows:** `%LOCALAPPDATA%\SteamClip\`
- **Linux:** `~/.config/SteamClip\`

---

## Contributing

Bug reports and feature requests go in [Issues](https://github.com/Nastas95/SteamClip/issues)

---

## YouTube Integration

SteamClip can upload clips directly to YouTube. This requires a Google Cloud project with the YouTube Data API v3 enabled.

### Setup

**1. Create a Google Cloud project**

1. Go to [Google Cloud Console](https://console.cloud.google.com/) and create a new project (or use an existing one)
2. In the left sidebar, navigate to **APIs & Services → Library**
3. Search for **YouTube Data API v3** and click **Enable**

**2. Create OAuth 2.0 credentials**

1. Go to **APIs & Services → Credentials**
2. Click **Create Credentials → OAuth client ID**
3. If prompted, configure the OAuth consent screen first — set it to **External**, fill in the app name, and add your Google account as a test user
4. For application type, select **Desktop app**
5. Download or note your **Client ID** and **Client Secret**

**3. Set environment variables**

Set these two variables in your environment before launching SteamClip:

| Variable | Value |
|---|---|
| `YOUTUBE_CLIENT_ID` | Your OAuth 2.0 client ID |
| `YOUTUBE_CLIENT_SECRET` | Your OAuth 2.0 client secret |

**Windows (PowerShell):**
```powershell
$env:YOUTUBE_CLIENT_ID = "your-client-id"
$env:YOUTUBE_CLIENT_SECRET = "your-client-secret"
python steamclip.py
```

**Windows (persistent, via System Properties → Environment Variables):** add both variables under User variables

**Linux / macOS:**
```bash
export YOUTUBE_CLIENT_ID="your-client-id"
export YOUTUBE_CLIENT_SECRET="your-client-secret"
python steamclip.py
```

### First upload

On the first upload SteamClip will open your browser for Google sign-in and request permission to upload videos on your behalf. After you approve, a token is saved to the config folder so future uploads do not require re-authentication.

- **Windows:** `%LOCALAPPDATA%\SteamClip\youtube_token.json`
- **Linux:** `~/.config/SteamClip/youtube_token.json`

Delete that file to revoke access or force re-authentication.

---

*Developed with ❤️ and a little AI assistance for the Steam Deck and PC gaming community*
