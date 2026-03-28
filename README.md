<div align="center">

# J.A.R.V.I.S.

### Just A Rather Very Intelligent System

*Your AI butler for the terminal — Iron Man-style voice notifications for Claude Code*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/Node.js-%3E%3D18-43853d.svg)](https://nodejs.org/)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-0-brightgreen.svg)](#)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Hooks-7c3aed.svg)](https://docs.anthropic.com/en/docs/claude-code/hooks)

<br>

**Jarvis speaks to you** when tasks complete, permissions are needed, errors occur, and more.

*"All systems online, sir."*

<br>

---

</div>

<br>

## Overview

Jarvis plugs into [Claude Code hooks](https://docs.anthropic.com/en/docs/claude-code/hooks) to deliver real-time voice notifications during your coding sessions. It announces task completions with elapsed time, alerts you to permission prompts so you never miss them, reports build and test results, and greets you with dry British wit — all without ever blocking your workflow.

<br>

## Quick Start

```bash
# Install hooks into Claude Code
node jarvis/bin/jarvis.mjs install

# Restart Claude Code — Jarvis is now active

# Test voice output
node jarvis/bin/jarvis.mjs test
```

That's it. Jarvis will greet you on your next session.

<br>

## Features

<table>
<tr>
<td width="50%">

#### Voice Notifications
Get spoken alerts for task completions, errors, permission requests, and session events — no more staring at the terminal waiting.

</td>
<td width="50%">

#### Smart Filtering
Only announces what matters: agent dispatches, build results, and critical events. Ignores routine file reads and simple commands.

</td>
</tr>
<tr>
<td width="50%">

#### Multi-Platform TTS
Works everywhere — premium AI voice via ElevenLabs, native system TTS on macOS/Linux/Windows, or simple stderr fallback.

</td>
<td width="50%">

#### Zero Dependencies
Pure Node.js with no external packages. Lightweight, fast, and nothing to break.

</td>
</tr>
<tr>
<td width="50%">

#### Jarvis Personality
Dry British wit, formal but warm, occasionally sarcastic. Every message is crafted with character.

</td>
<td width="50%">

#### Non-Blocking Design
Always exits with code 0. Never blocks Claude Code, even if speech synthesis fails entirely.

</td>
</tr>
</table>

<br>

## Events

Jarvis listens to the full Claude Code lifecycle:

| Event | What Jarvis Says |
|:---|:---|
| **SessionStart** | Greeting — *"All systems online, sir."* |
| **SessionEnd** | Farewell — *"Signing off, sir."* |
| **Stop** | Task summary with tool count and elapsed time |
| **StopFailure** | API error announcement |
| **Notification** | Permission prompt *(debounced to 10s)* |
| **PermissionRequest** | Tool-specific approval request |
| **PreToolUse** | Agent dispatch announcement |
| **PostToolUse** | Build/test result summary |
| **PostToolUseFailure** | Error description |
| **SubagentStop** | Sub-agent completion |
| **PreCompact** | Long-running task status update |

<br>

## Voice Engines

Jarvis selects the best available voice engine automatically:

```
 1.  ElevenLabs API     Premium AI voice (requires API key)
 2.  System TTS         Platform-native fallback
      ├─ macOS           say command (Daniel voice, rate 180)
      ├─ Linux           espeak-ng → espeak → spd-say → piper
      └─ Windows         PowerShell SAPI
 3.  Stderr             Messages print to terminal if no TTS available
```

<br>

## Configuration

All settings via environment variables — no config files needed:

| Variable | Default | Description |
|:---|:---|:---|
| `ELEVENLABS_API_KEY` | — | ElevenLabs API key for premium voice |
| `JARVIS_VOICE_ID` | `onwK4e9ZLuTAKqWW03F9` | ElevenLabs voice ID (Daniel) |
| `JARVIS_MODEL` | `eleven_turbo_v2_5` | ElevenLabs model |
| `JARVIS_MACOS_VOICE` | `Daniel` | macOS `say` voice name |
| `JARVIS_MACOS_RATE` | `180` | macOS `say` speech rate |

<br>

## CLI

```
jarvis install       Install Claude Code hooks
jarvis uninstall     Remove Claude Code hooks
jarvis test          Test voice engine with a greeting
jarvis status        Show current configuration
jarvis say <text>    Speak arbitrary text
jarvis help          Show help
```

<br>

## Architecture

```
jarvis-claude-code/
├── bin/
│   ├── jarvis.mjs          CLI entry point
│   ├── install.mjs         Hook installer
│   └── uninstall.mjs       Hook uninstaller
├── hooks/
│   └── jarvis-hook.mjs     Unified hook handler (all events)
├── src/
│   ├── voice.mjs           TTS engine (ElevenLabs + system fallback)
│   └── personality.mjs     Message generator (Jarvis-style wit)
└── package.json
```

**Design principles:**

- **Single hook handler** — One script handles all events, reading the event type from `argv[2]` and payload from stdin
- **State tracking** — Temp file (`/tmp/jarvis-state.json`) persists session time, tool count, and debounce timestamps across invocations
- **Debounced notifications** — Throttled to once per 10 seconds to prevent notification spam
- **Graceful degradation** — Voice engine cascade ensures something always works

<br>

## Installing TTS on Linux

```bash
# Recommended
sudo apt install espeak-ng

# Alternatives
sudo apt install espeak
sudo apt install speech-dispatcher
```

<br>

---

<div align="center">

**Part of the [Paperclip](https://github.com/roy-substrate/paperclip) project**

MIT License

</div>
