# Sanchez-Ai

A single-file, Gemini-powered Rick Sanchez chatbot with a real-time animated CSS face, multi-chat history, and optional Rick Sanchez voice cloning via Fish Audio.

No build step, no backend (aside from an optional voice proxy) — open the HTML file and go.

**Note**- There are limitations to the free API on both gemini and fish audio , especially with fish audio where  the audio may stop mid sentence because the limits are reached,and sometimes the gemini free  API migh be busy or down

---

## Features

- **Animated Rick face** — pure CSS/HTML portrait (no images, no canvas) that blinks on its own and reacts with expressions (`talking`, `angry`, `smirk`, `thinking`, `sleep`, `surprised`, `manic`, `disgust`, `exasperated`) driven by the content of Rick's replies.
- **Multi-chat history** — create, switch between, and delete separate conversations, saved to `localStorage`.
- **Gemini-powered responses** — Rick is fully in-character: brilliant and helpful for genuinely interesting questions, dismissive and insulting for trivial ones.
- **Optional Google Search grounding** — add a Custom Search Engine (CX) ID alongside your API key and Rick will pull in live results for time-sensitive questions before answering.
- **Voice** — three modes: browser speech-to-text for input, browser text-to-speech for output (no key needed), or Rick's actual cloned voice via Fish Audio through a small Cloudflare Worker proxy.
- **Guided setup tour** — walks first-time users through getting a free Gemini API key.

## Getting started

1. Open `rick-ai.html` in a browser (Chrome or Edge recommended for voice input).
2. Follow the on-screen tour, or skip it and get a free key yourself at [aistudio.google.com/apikey](https://aistudio.google.com/apikey).
3. Paste the key into the **GOOGLE API** field at the top of the chat and hit **CONNECT**.
4. Start typing. That's it.

Your API key is only ever held in memory in that browser tab — it's never sent anywhere except directly to Google, and it's never stored.

## Model selection

The model dropdown in the top-right defaults to **Gemini 2.5 Flash** and includes the full current Gemini lineup available on the free tier, plus a few paid-only options for anyone with billing enabled:

| Model | Free tier |
|---|---|
| Gemini 2.5 Flash *(default)* | 5 RPM |
| Gemini 2.5 Flash Lite | 10 RPM |
| Gemini 2 Flash | paid only |
| Gemini 2 Flash Lite | paid only |
| Gemini 2.5 Pro | paid only |
| Gemini 3 Flash | 5 RPM |
| Gemini 3.1 Pro | paid only |
| Gemini 3.1 Flash Lite | 15 RPM |
| Gemini 3.5 Flash | 5 RPM |

Switch models any time from the dropdown — no reconnect needed.

## Voice setup (optional)

Click **VOICE** in the left panel to choose a mode:

- **Voice input only** — speech-to-text into the chat box, no key required.
- **Rick's real voice (Fish Audio)** — requires a free [Fish Audio](https://fish.audio) API key and the Cloudflare Worker proxy described below.
- **Browser voice** — built-in TTS, pitched down and slowed to approximate Rick. No key required.

### Why the proxy?

Fish Audio's API is server-to-server only — it doesn't allow direct browser calls (CORS blocks any cross-origin request carrying an `Authorization` header). The fix is a ~60-line Cloudflare Worker that sits between the browser and Fish Audio: the browser calls the Worker on your own domain (no CORS issue), the Worker forwards the request to Fish Audio with your key, and streams the audio back with open CORS headers. Deploys free from the Cloudflare dashboard in a couple of minutes.

## File structure

Everything lives in one file: `rick-ai.html`. HTML, CSS, and JavaScript are all inline — nothing to install, nothing to build.

## Known limitations

- Voice input requires a Chromium-based browser (`webkitSpeechRecognition`).
- Chat history and voice mode are stored in `localStorage`, so they're per-browser, not synced across devices.
- Web search grounding requires a separate Google Custom Search Engine ID in addition to the Gemini key.
