# GlassBox

**Every measurement a website can take from your browser, run live and shown back to you.**

GlassBox is a client-side browser-fingerprinting bench. It runs ~29 families of fingerprinting and device-enumeration probes against *your own* browser, shows you the raw values, and estimates how identifiable you are. These are the same signals tracking and anti-fraud scripts collect — surfaced instead of hidden.

🔗 **Live demo:** https://glassbox.codecanary.org
📖 **Anonymization guide:** https://glassbox.codecanary.org/guide

> No screenshots here on purpose: GlassBox displays the viewer's own IP, location, and hardware, so any real screenshot would leak a real person's fingerprint. Run the live demo to see it.

## What it measures

- Navigator core, UA client hints, screen & display, timezone & locale
- Canvas 2D, WebGL / WebGL2, WebGPU, and audio-stack fingerprints
- Installed fonts, media codecs, speech-synthesis voices, media devices
- WebRTC / IP leak, storage & quota, permissions matrix, API-support matrix
- CSS & media features, JS-engine math, timing / compute, keyboard layout, battery
- Automation / bot signals, cookies, a locally-reconstructed tracking pixel, and a fingerprint-resistance check
- Opt-in: public IP intelligence (geo / ASN / VPN detection), behavioral capture, and cross-site login-state

Signals fold into four tiers — hardware, engine × hardware, browser build, and session — plus a headline **estimated identifiability** score.

## Privacy

Almost everything runs in your browser and stays there: **no analytics, no beacon, nothing is sent.** The one exception is opt-out IP intelligence, which queries public geolocation APIs (ipwho.is, ipapi.is) for your address / network / VPN status — toggle **Geo** off to stay fully local. View source to confirm the rest.

### About the identifiability %

The headline percentage is an honest **model, not a live-population measurement** — a no-server tool can't compute true rarity against real visitors. It sums published per-signal entropy (Panopticlick, AmIUnique, EFF Cover Your Tracks), counts only what your browser actually exposes (masked canvas / GPU are discounted), applies a correlation discount, and caps at the ~33 bits needed to single out one person among ~8 billion. Treat it as an order-of-magnitude indicator. One honest wrinkle: a browser that blends into a big crowd (Tor Browser at its default size) is *safer* than its bit-count suggests, because everyone there reports the same values. For numbers measured against a live population, compare with [EFF Cover Your Tracks](https://coveryourtracks.eff.org) and [AmIUnique](https://amiunique.org).

## Run it

A single static file — no build step, no dependencies. Any static host works:

```bash
# locally
python3 -m http.server 8000      # then open http://localhost:8000

# Cloudflare Pages
npx wrangler pages deploy . --project-name=glassbox
```

Host it at its own `https://` origin for the full surface — some probes (WebRTC, media devices, permissions, login-state) are limited on `file://` or inside an embedded frame.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The app — all probes plus the identifiability estimate, fully self-contained |
| `guide.html` | Anonymization how-to guide (served at `/guide`) |
| `_headers`   | Security headers for Cloudflare Pages |

## Prior art & credit

Inspired by and worth comparing to [EFF Cover Your Tracks](https://coveryourtracks.eff.org), [AmIUnique](https://amiunique.org), Panopticlick, and [browserleaks.com](https://browserleaks.com). Built as part of [CodeCanary](https://codecanary.org).

## License

[MIT](LICENSE) © HotStart Labs
