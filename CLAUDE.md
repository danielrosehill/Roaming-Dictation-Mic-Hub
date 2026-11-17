# Roaming Dictation Mic Hub

Public idea repo, created 2026-11-17 from Daniel's voice note. The concept: the phone is the one
dictation mic, and a central server routes its audio stream to whichever client is selected (local
desktop or laptop, or a remote machine over Tailscale). Each client runs the local ASR (Hebrish)
fed from a network stream and types the result.

**Status (2026-11-17):** parked idea. No code. Version 1 (phone streams straight to receivers,
no server) is a separate private prototype. Don't name or link it here: this repo is public.

## Where things are

- `README.md`: the cleaned voice note (the brief) plus open tasks.
- `context/voice-notes/2026-11-17-roaming-mic-hub-v2/`: `voice-note.mp3` (cleaned with Sidon v0.1
  on Modal, loudness-normalised), `transcript-raw.txt` (AssemblyAI, unedited, never edit it),
  `transcript-timed.txt`, `processing.json` (provenance).

## Rules from the note

- Wi-Fi/LAN streaming, not Bluetooth retransmission (Daniel ruled Bluetooth relay out).
- The server owns client management. The phone is a controller, not the streaming server.
- The main use case is a phone (mobile) as sender. Desktop-as-sender is marginal.
- Do a prior art review before building.

Source-of-truth order: the voice note (raw transcript), then README.
