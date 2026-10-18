<div align="center">

<img src="assets/banner.png" alt="Muesli for Obsidian: the AI notepad for back-to-back meetings, without a meeting bot">

### Granola, in Obsidian. Record any call, get notes, actions and a transcript in your vault.

Free and open source. No bot joins your call. No subscription.

[![Obsidian community plugin](https://img.shields.io/badge/Obsidian-community%20plugin-7c3aed)](https://community.obsidian.md/plugins/muesli)
[![License: MIT](https://img.shields.io/badge/license-MIT-333)](LICENSE)
![macOS | Windows | Linux | iOS | Android](https://img.shields.io/badge/macOS%20%7C%20Windows%20%7C%20Linux%20%7C%20mobile-333)

<table>
  <tr>
    <td align="center" width="200"><h2>0</h2>bots in your call</td>
    <td align="center" width="200"><h2>$0.10</h2>per hour of speech</td>
    <td align="center" width="200"><h2>Seconds</h2>from stop to notes</td>
    <td align="center" width="200"><h2>100%</h2>plain Markdown, yours</td>
  </tr>
</table>

**English** · [Français](README.fr.md)

</div>

## How it works

1. Click 🎙️ → **Meeting**.
2. Jot a few words in `Notes.md` while people talk.
3. Click stop. Seconds later:

```
Meetings/2026-10-09 1430 Pricing review with Acme/
  Summary.md      ← TL;DR, decisions, actions, open questions
  Transcript.md   ← who said what, with timestamps
  Notes.md        ← your raw notes, untouched
  audio.mp3       ← optional
```

Actions come out as `- [ ] Paul — send the recap (Friday)`, ready for Tasks, Dataview and Bases.

## Install

In Obsidian: **Settings → Community plugins → Browse → "Muesli" → Install → Enable**.
Or [open it in the directory](https://community.obsidian.md/plugins/muesli) and click *Add to Obsidian*.

A setup screen then asks for an [OpenRouter key](https://openrouter.ai/keys) (pay per use, a few dollars last months), your name, and runs a sound test. Done.

## Why Muesli

|                                   | Granola | Otter / Fireflies | **Muesli** |
|-----------------------------------|:-------:|:-----------------:|:----------:|
| No bot in the call                |    ✓    |         –         |   **✓**    |
| Notes in your Obsidian vault      |    –    |         –         |   **✓**    |
| No subscription, pay per use      |    –    |         –         |   **✓**    |
| Open source                       |    –    |         –         |   **✓**    |
| Works with Zoom, Meet, Teams, any app |  ✓  |         ~         |   **✓**    |

## Also does

- **Voice journal**: speak your tasks, plans and mood, get bullets in today's daily note.
- **Import any recording**: drop an `.m4a` or `.mp3` in `Meetings/Inbox`, get the same folder.
- **iPhone Voice Memos**: new memos are transcribed and filed on your Mac, automatically.
- **On your phone**: in-person meetings and journal, one big button.
- **Your template, your language**: summaries follow your own templates (General, 1:1, sales call…), in English or French.
- **Never forget to stop**: a small pill over every app, hidden from screen shares, ends forgotten meetings by itself.

## Privacy and disclosures

- **Network**: audio and text go **only to [OpenRouter](https://openrouter.ai)** (and the model providers it routes to), with your own key. No telemetry, no server of ours.
- **Paid service**: OpenRouter is pay-per-use (about $0.10 per hour of speech, plus a few cents of LLM per meeting).
- **Native helper (macOS)**: system audio is captured by `meeting-tap`, a small open-source Swift program ([source](native/macos/main.swift)) embedded in `main.js` with its SHA-256 and written to the plugin folder on first use. Nothing is downloaded. Both are built by GitHub Actions with [build provenance attestations](https://github.com/Azilone/obsidian-muesli/attestations).
- **Files outside the vault**: only when you import a file from disk, or with the optional Voice Memos inbox (needs Full Disk Access).
- **Always tell participants you are recording**, and follow the consent laws where you live.

<details>
<summary><b>Requirements</b></summary>

| | Calls (mic + system audio) | In person, journal, import |
|---|---|---|
| macOS 14.4+ | ✅ | ✅ |
| Windows, Linux | — (mic only for now) | ✅ |
| iOS, Android | — | ✅ |

Obsidian 1.13 or later, and an [OpenRouter API key](https://openrouter.ai/keys) with a few dollars of credit.
On iOS, keep Obsidian in front while recording. The key is stored per device.

</details>

<details>
<summary><b>Other ways to install</b></summary>

- **BRAT** (latest builds): install [BRAT](https://github.com/TfTHacker/obsidian42-brat), then *BRAT: Add a beta plugin* → `Azilone/obsidian-muesli`.
- **Manually**: download `main.js`, `manifest.json` and `styles.css` from the [latest release](https://github.com/Azilone/obsidian-muesli/releases/latest) into `<your vault>/.obsidian/plugins/muesli/`, then enable **Muesli**.
- **From source**: see [CONTRIBUTING.md](CONTRIBUTING.md#development).

</details>

<details>
<summary><b>Under the hood</b></summary>

| Piece | What it does |
|---|---|
| **Microphone** | Web Audio. In a call, it is *you*, so you are always attributed correctly. |
| **System audio** | `meeting-tap` (Swift, ~320 KB) taps the Mac's audio output through Core Audio process taps. No bot, no virtual audio driver. |
| **Silence removal** | An on-device voice detector strips silences before upload, cutting chunks inside silences so no word is split. |
| **Transcription** | [`microsoft/mai-transcribe-2`](https://openrouter.ai/microsoft/mai-transcribe-2) through OpenRouter, with speaker diarization, in the background *during* the meeting. |
| **Clean-up and summary** | `anthropic/claude-haiku-5.5` by default (any OpenRouter model works): matches speakers, fixes errors, writes the summary around your notes, picks a title. |

Everything is written to your vault first (`<meeting>/.raw/`), so a crash or a failed API call never loses a meeting. **Re-process the meeting** picks up where it stopped.
Syncing your vault with git? Consider adding `**/.raw/` and `*.mp3` to `.gitignore`.

</details>

<details>
<summary><b>Settings</b></summary>

| Setting | Default |
|---|---|
| Default meeting type | Call (mic + system audio) / In person (mic only) |
| Default summary template | `General` |
| Language / summary language | Auto-detect / English (French if Obsidian is in French) |
| Keep the audio | On |
| Floating overlay | On, hidden from screen sharing |
| Ask after N min without speech / stop after | 10 min / 5 min |
| Maximum duration | 180 min |
| Models | `microsoft/mai-transcribe-2`, `anthropic/claude-haiku-5.5` |
| Folders | `Meetings`, `Meetings/Templates`, `Meetings/Inbox`, `Voice notes` |

</details>

<details>
<summary><b>Development</b></summary>

```bash
bun install
bun run check                        # type-check, Obsidian lint rules, tests
./native/build.sh                    # rebuild native/meeting-tap (universal, ad-hoc signed)
VAULT="/path/to/vault" bun run dev   # watch mode, builds straight into the vault
```

The helper writes raw PCM (s16le, mono, 16 kHz) on stdout and JSON status lines on stderr. Any helper that follows this contract can be added for Windows (WASAPI loopback) or Linux (PipeWire monitor): contributions welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

</details>

## Roadmap

- Windows and Linux system audio
- Live transcript while recording
- "Ask your meetings" chat across the vault
- Title and participants from the calendar

<div align="center">

**If Muesli saved you a meeting recap, a ⭐ helps others find it.**

[MIT](LICENSE)

</div>
