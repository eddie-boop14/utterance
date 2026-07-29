# utterance

**Ephemeral peer-to-peer messaging in a single HTML file. Proven · and gone.**

No server of ours. No account. No database. No storage APIs at all — close the tab and the
conversation has never existed anywhere but in the room. One file you can read end to end.

## Try it

Open the page, share the room link with one person, talk. That's the product.

## How it works

- **Transport:** WebRTC data channels, browser-native DTLS encryption end to end.
- **Rooms:** 96-bit random IDs (`crypto.getRandomValues`) — unguessable, unenumerable.
- **Authenticity (optional, per message):** toggle signing (Tab or the sigil) and each
  message carries an ECDSA P-256 signature bound to the room, the text, a timestamp and a
  one-time nonce — replayed or cross-room signatures are rejected.
- **Voiceprint:** your signing key's SHA-256 renders as three words ("ember · lagoon ·
  ninth"). Speak yours aloud; tap any ◆ mark to reveal the sender's. If what they see
  matches what you say, no one sits between you.
- **Panic:** Escape — or triple-tap the footer — instantly empties the room on your device.
- **Connectivity:** STUN (Google, Cloudflare) for NAT traversal; TURN relay (OpenRelay)
  as fallback when direct connection fails. The relay carries only DTLS-encrypted bytes.
- **Persistence:** none. No localStorage, no sessionStorage, no IndexedDB, no cookies.
  Messages exist only on screen, and once you're alone in the room they fade away on
  their own within seconds. Nothing survives the tab; most things don't survive the
  minute.

## Threat model — read this before trusting it

Honesty over marketing. What each party can and cannot see:

| Party | Sees | Cannot see |
|---|---|---|
| **PeerJS signaling server** (public `0.peerjs.com` cloud) | that two peer IDs connected, when, from which IPs | message content |
| **TURN relay** (OpenRelay, when used) | encrypted traffic volume and timing | message content |
| **Your peer** | everything you send | anything after the tab closes |
| **Us** | nothing — there is no us; no server of ours exists | — |

Known limitations, stated plainly:

1. **Metadata is not hidden.** The signaling server learns that a conversation happened
   and between which IPs. If your threat model includes hiding *that you talked*, use Tor
   or don't use a browser tool.
2. **Key exchange is in-band — but MITM is now detectable.** Keys travel with signed
   messages, so a malicious signaling server could in principle interpose. The voiceprint
   exists exactly for this: compare the three words out loud (or over any second channel)
   and an impostor key is exposed immediately. Unsigned (anon) messages carry no key and
   remain unauthenticated by design.
3. **No forward secrecy claims.** Session keys are fresh per session and never stored,
   but this is not a Signal-grade ratchet and doesn't pretend to be.
4. **The other person's device is out of scope.** Ephemeral means *we* keep nothing;
   screenshots exist.

If your life depends on it, use Signal. If you want a room that provably keeps no record
and that you can audit in one file, this is that.

## Self-hosting

It's one HTML file. Host it anywhere static. To also own the metadata layer, run your own
PeerServer and change the host in the config — then no third party sees even the
connection events.

## Why

Because "no logs" should be an architecture, not a promise.

*© bleu-canard éditions · Edmaster & Claudius 🦆 · MIT*
