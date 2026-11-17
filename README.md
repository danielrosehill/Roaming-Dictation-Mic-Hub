# Roaming Dictation Mic Hub

The idea: use your phone as your one dictation microphone all day. A home server routes the phone's
audio to whichever computer you're working at, or to a remote machine over Tailscale, where it is
typed as if from a local keyboard.

**Status (2026-11-17):** idea, parked for future exploration. Nothing built here yet. A first
prototype (phone streams straight to the desktop, without a server) exists as a private APK that
hasn't been tested on a phone yet.

## The voice note

🎧 [Listen to the original voice note](context/voice-notes/2026-11-17-roaming-mic-hub-v2/voice-note.mp3)
(8:56, recorded 2026-11-17, cleaned with [Sidon](https://huggingface.co/sarulab-speech/sidon-v0.1)).
Raw transcript: [transcript-raw.txt](context/voice-notes/2026-11-17-roaming-mic-hub-v2/transcript-raw.txt).

What follows is that note, lightly edited: fillers and false starts removed, mishearings fixed (listed
at the end). The wording is still mine.

### The problem

This is my voice technology idea, and I think it would be very cool. I'll explain my first
iteration, what I'm thinking for a second one, and the pros and possible tripping points I've just
thought of.

At home I use dictation all day, every day. It's fantastic, and it's now the only way I use my
computers. But during the day I go out, come back in, and use different devices.

### Version 1: the phone streams to each computer

For the first prototype I thought: why not use my phone? It's what I carry with me all day. The
basic mode is dictating into the phone itself, which doesn't really need an app. Call it the
localhost route.

Then there's a client on each computer, so there are a few destinations: desktop and laptop, say.
The phone streams its microphone to those devices, each running a client. If I move over to my
desktop, I toggle a button in the app that says "desktop", and my microphone is now streamed to the
desktop and bypasses the phone. That matters because if dictated text were typed on several devices
at once, the phone would be unusable.

### Version 2: a central server routes the stream

What I thought about today is that it probably makes more sense to have a central server that
passes the stream through. The phone still runs the app (the controller app, let's call it). If the
destination is the phone itself, nothing needs to go to the server; anything going somewhere else
goes through the server, and the server streams it on.

That way you manage clients at the server level. Having the phone client also be the streaming
server is a very brittle design.

### Remote destinations over Tailscale

If everything goes via the server, and the server has a Tailscale route to a VPS or a computer on
another network, you could use the same microphone not only to send your dictation to different
local devices but also to a remote device, as a remote virtual keyboard.

It's not a simple interface or idea. It's one of the experiments I've been thinking about for a
while, but it would be very cool.

### Why Wi-Fi, not Bluetooth

I'm not a big fan of Bluetooth microphones for dictation. Recently I've been using USB-C microphones
with Android, and I'm surprised how well that works.

The more conventional approach would be a Bluetooth headset with multipoint or Auracast, toggling
where it sends audio. That's a simpler setup. My original architecture had the server act as a
Bluetooth receiver and retransmit over Bluetooth. Thinking aloud while I recorded this, I realised
that's a really dumb architecture: Wi-Fi is much more reliable than Bluetooth for moving audio
around the local network.

### Use cases

1. **Phone in hand.** Your phone is what you hold all day. I get very good results just holding it
   up to my mouth, like I'm doing now. It's the quick and dirty way to get decent input for ASR. In
   the app you toggle to a destination, the audio goes through the server (a local proxy), and
   arrives at whatever device you're on.
2. **Wired headset, phone in your pocket.** You move over to your laptop, switch to "laptop", and the
   audio streams through the server to the laptop's client, which runs the dictation.

That means every client device needs my local dictation model on it (currently Hebrish). Hebrish
would need to support a network stream as a microphone source, not just a local microphone.

### Streaming and latency

I can't remember whether Icecast is the right tool, or what the most robust, classic setup for
streaming audio is. For dictation, assume the audio reaches the phone from a Bluetooth headset, so it
won't be high quality or high bitrate. What it needs is to route that stream around reliably.

Sending to a remote machine adds latency, and the audio would travel through a Tailscale tunnel. It
may sound like a slightly crazy idea, but I think it could work with a bit of finagling and Claude
Code doing its magic.

### Why it matters

I dictate all day and swap between devices as my day changes: going in and out, sometimes working
from co-working spaces. This lets me say: my phone is my dictation device, and this app pops it out
to wherever I am. That's the hub-and-spoke approach, and that's the idea.

### Prior art

We should include a prior art review. I'd be curious to know whether anyone has built something
like this.

The hub-and-spoke model probably only makes sense with a mobile device as the sender. You could
also use a desktop as the streamer and use voice input on remote machines, but that's a much more
marginal use case than the one I'm envisioning.

### Next

I'll transcribe this and park it for future exploration. I already have the early APK prototype on
my phone and haven't tested it yet. Claude Opus 5.5 builds things faster than I can try them out,
which is kind of amazing. Testing that prototype will be a good first check that this is viable. I
recorded this while the idea is fresh, because a server in the middle could be the more elegant way
to architect it.

### Corrections

- "Hebrush" → **Hebrish** (my local dictation model).
- "oracast" → **Auracast** (Bluetooth LE Audio broadcast).
- "Claude co" → **Claude Code**.

## Open tasks

- [ ] Test the version 1 APK on a phone (first check that the idea is viable).
- [ ] Prior art review: phone-as-network-mic apps, audio routing over a LAN or Tailscale, remote
      dictation and virtual-keyboard tools.
- [ ] Choose a streaming transport for low-bitrate speech over LAN and Tailscale (Icecast, RTP/Opus,
      WebRTC, raw TCP) based on latency and reliability.
- [ ] Hebrish: support a network stream as a microphone source.
