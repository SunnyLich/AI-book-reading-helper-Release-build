# Context Reader (Anitigravity Workspace)

Desktop reading app built with React, TypeScript, Vite, and Electron.

Import long-form text, read in scroll or book mode, highlight important terms, and use local AI plus dictionary tools to understand content faster.

## Features

- Text import from PDF, EPUB, TXT, MD, pasted text, and AO3 links
- Two reading modes:
  - Scroll mode for continuous reading
  - Book mode with two-page layout and page-turn audio
- Keyword workflow:
  - Manual keyword editing (term, definition, highlight color)
  - Auto keyword extraction from visible context (Ollama)
  - Import/Export keywords as JSON
  - Keyword search and jump-to-row in editor
  - Alphabetical keyword ordering for easier lookup
- Selection tools:
  - Explain selected text with Ollama
  - Translate selected text with Ollama (quick + advanced)
  - Dictionary-based translation for installed dictionary packs
- Localization and settings:
  - Multi-language UI support
  - Persistent reader settings (font scale, language, etc.)
- Desktop integrations:
  - Electron IPC bridge for explanation jobs, find-in-page, dictionary storage, and health checks

## Tech Stack

- React 19 + TypeScript
- Vite 7
- Electron 34
- pdfjs-dist for PDF extraction
- JSZip for EPUB parsing
- Ollama (optional, for AI explanation/translation)

## Project Structure

- src: React renderer app
- src/components: Reader, importer, keyword UI
- electron/main.cts: Electron main process, queueing, Ollama + IPC handlers
- electron/preload.cts: Secure bridge exposed to renderer
- public/dictionaries: Bundled dictionary pack files
- scripts/package-win.cjs: Windows packaging helper

## Prerequisites

- Node.js 20+ recommended
- npm 10+ recommended
- Windows/macOS/Linux for development
- Ollama installed locally if you want AI explanation/translation features

## Getting Started

1) Install dependencies

```bash
npm install
```

2) Run web dev mode

```bash
npm run dev
```

3) Run Electron desktop dev mode

```bash
npm run electron:dev
```

## Build

Build renderer + Electron TypeScript outputs:

```bash
npm run build
```

Build packaged desktop app with electron-builder:

```bash
npm run electron:build
```

Create a Windows unpacked package with electron-packager:

```bash
npm run package-win
```

## Ollama Notes

AI-based features (keyword extraction, explanations, translation) call a local Ollama instance.

- Default endpoint: http://127.0.0.1:11434
- The app checks health and attempts startup flows from Electron main process
- If Ollama is unavailable, dictionary/manual keyword workflows still function

## Keyword Import/Export JSON Format

Export creates a JSON file shaped like:

```json
{
  "format": "keyword-list",
  "version": 1,
  "exportedAt": "2026-02-28T00:00:00.000Z",
  "keywords": [
    {
      "id": "...",
      "term": "example",
      "definition": "example definition",
      "color": "#285fd4"
    }
  ]
}
```

Import accepts either:

- A top-level array of keyword objects, or
- An object with a keywords array

Import behavior:

- Merge by term (case-insensitive)
- Imported definition/color overwrite existing matching terms
- Invalid entries are skipped with summary feedback

## Scripts

- npm run dev: Start Vite dev server
- npm run electron:dev: Start Vite + Electron in development
- npm run lint: Run ESLint
- npm run build: Build renderer and Electron TS output
- npm run electron:build: Build packaged app via electron-builder
- npm run package-win: Build Windows package via electron-packager

## Current Status

This project is actively evolving. Core reader, import pipeline, keyword management, translation flows, and Electron architecture are implemented and usable.

## License

No license file is currently included.
