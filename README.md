#  SoundWave

A free, self-hosted music streaming web app — search, stream, download, and save songs for offline listening, powered by YouTube as the audio source.

## Overview

SoundWave is a lightweight Spotify-style music player made of two pieces:

- **Frontend** — a single-page `index.html` (vanilla JS, no build step) with a sidebar, search page, queue, offline library, and a full audio player bar (play/pause, shuffle, repeat, seek, volume).
- **Backend** — a Flask API that uses [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) to search YouTube, resolve direct audio stream URLs, fetch trending tracks, generate simple seed-based playlists, and download tracks as MP3.

Downloaded songs are cached on the server (`audio_cache/`) and saved client-side in **IndexedDB**, so they can be played back with no internet connection.

## Features

- 🔍 Search any song/artist via YouTube
- ▶️ Stream instantly (no full download needed to play)
- ⬇️ Download tracks as MP3 for offline use
- 📥 Offline library backed by IndexedDB
- 🔀 Shuffle, 🔁 repeat (off/all/one), queue management ("Up Next")
- Trending page and genre quick-search tags (Lo-Fi, Pop, Hip Hop, Bollywood, etc.)
- Basic auto-playlist generation from seed songs
- Dark, glassmorphism-style UI with no external UI framework

## Tech Stack

| Layer | Tech |
|---|---|
| Frontend | HTML, CSS, vanilla JavaScript, IndexedDB |
| Backend | Python, Flask, Flask-CORS |
| Audio source | yt-dlp (YouTube search/stream/download) |
| Deployment | Nixpacks (`ffmpeg` + `python3.11`), Procfile for Railway/Heroku-style hosts |

## Project Structure

```
soundwave-_a_music_platform/
├── backend/
│   ├── app.py           # Flask API (search, stream, download, trending, playlist)
│   ├── requirements.txt
│   ├── Procfile          # web: python app.py
│   ├── runtime.txt       # python-3.11.0
│   ├── nixpacks.toml     # deployment build config
│   └── audio_cache/      # cached MP3 downloads
└── frontend/
    └── index.html        # entire UI + player logic
```

## Getting Started

### Backend

```bash
cd backend
pip install -r requirements.txt
python app.py
```

The API runs on `http://localhost:5000`. It needs `ffmpeg` available on your system for audio conversion.

### Frontend

Just open `frontend/index.html` in a browser (it talks to the backend at `http://localhost:5000/api`). No build tools required.

## API Endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/api/search?q=<query>&limit=<n>` | GET | Search YouTube for songs |
| `/api/stream/<video_id>` | GET | Get a direct audio stream URL |
| `/api/download/<video_id>` | GET | Download/serve a track as MP3 |
| `/api/info/<video_id>` | GET | Get metadata for a track |
| `/api/playlist/generate` | POST | Generate a playlist from seed songs |
| `/api/trending` | GET | Get currently trending tracks |
| `/api/health` | GET | Check backend + yt-dlp status |

## Deployment

The included `Procfile` and `nixpacks.toml` are set up for one-click deploys to platforms like Railway or Render. Just push the `backend/` folder as the service root and serve `frontend/index.html` as a static site (updating the `API` constant in `index.html` to point at your deployed backend URL).

##  A Note on Content

This project streams and downloads audio from YouTube via `yt-dlp`. Doing so may violate YouTube's Terms of Service and, depending on the content and your jurisdiction, copyright law. This is best treated as a personal/educational project rather than a public product — check the ToS and licensing implications before deploying it for others to use.

## License

No license specified yet — add one (MIT is a common choice for personal projects) if you plan to make this public.
