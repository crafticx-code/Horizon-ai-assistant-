# HORIZON AI ASSISTANT

A Jarvis-inspired voice and hand-gesture AI assistant that runs in a single HTML file. Talk to it in English or Hindi, steer its holographic planet with your hands, and let it open apps, start calls, and set timers. Bring your own Claude or DeepSeek API key.

**Live demo:** https://crafticx-code.github.io/Horizon-ai-assistant-/

> Horizon is an independent project inspired by science-fiction AI assistants. It is not affiliated with any film studio or brand.

## Features

- **Voice in English and Hindi.** Speak or type, and Horizon answers out loud. Hindi replies are written in Devanagari and spoken with a Hindi voice.
- **Hands-free mode.** Say "Hey Horizon" (or "हे होराइज़न") while the page is open.
- **Hand control with the camera.**
  - **Left hand:** move it closer to zoom in, pull it back to zoom out.
  - **Right hand:** move it and the planet follows that direction.
  - **Right pinch held:** resets the zoom.
  - **Right open palm held still:** starts listening.
  - **Fast swipe:** changes Horizon's state.
- **Holographic planet.** A canvas-rendered sphere with a connected surface network, data packets, HUD rings, a glowing core, and scanlines. It pulses on every spoken word and switches to a lighter mode on slow phones.
- **Actions.** Open apps (YouTube, Spotify, WhatsApp, Maps, Gmail, Instagram, Telegram, Netflix, Facebook, X, Chrome, Play Store, camera, settings), open websites, call saved contacts, send SMS or WhatsApp messages, set timers, and keep notes.
- **Permissions you control.** Switches for opening apps, calls and SMS, and websites. An "Ask me before acting" switch makes Horizon show a button and wait for your tap.
- **Your choice of AI.** Claude or DeepSeek, switchable in the app.
- **Five themes.** Gold, Arc Blue, Matrix Green, Crimson, and Violet.

## Quick start

1. Download `index.html`.
2. Open it from a secure address (`https://` or `localhost`). See "Run it" below.
3. Choose **Claude** or **DeepSeek** in the "AI" row.
4. Paste your API key and tap **Save key**.
5. Type `what time is it`, or tap **Talk to Horizon**.

## Run it

**In Acode (Android).** Save the file as `index.html`, tap the Run button, and set Acode's preview mode to open in your browser (Chrome). The in-app preview often blocks the mic and camera.

**On your computer.**

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

**Publish it free with GitHub Pages.**

1. Upload `index.html` to the top level of your repository.
2. Go to **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Open `https://crafticx-code.github.io/Horizon-ai-assistant-/` after about a minute.

Other static hosts such as Cloudflare Pages, Netlify, and Vercel work the same way. Mic and camera need HTTPS, which all of these provide.

## Project files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: interface, voice, hand tracking, planet, tools. |
| `deepseek-proxy-worker.js` | Optional Cloudflare Worker for DeepSeek if the browser is blocked (CORS). |
| `README.md` | This file. |

## Using DeepSeek

Choose **DeepSeek**, paste your key, and optionally set a model. The default is `deepseek-flash`; `deepseek-v4-pro` is the larger option. If Horizon says it cannot reach DeepSeek from the browser, DeepSeek may be blocking direct browser requests. In that case:

1. Create a free Cloudflare Worker and paste in `deepseek-proxy-worker.js`.
2. Deploy it.
3. Paste the worker URL into Horizon's **Proxy URL** box.

Your key travels from your device through the worker to DeepSeek. The worker does not store it.

## Permissions and contacts

| Switch | Default | What it allows |
| --- | --- | --- |
| Open apps | On | Opening the apps listed above. |
| Calls and SMS | **Off** | Calling contacts and sending messages. |
| Open websites | On | Opening websites. |
| Ask me before acting | On | Horizon shows a button, and you tap to confirm. |

When a permission is off, Horizon is not given that tool at all.

Add contacts in the Permissions panel, or tell Horizon to save one. Contacts are stored only in your browser. Ten-digit numbers starting with 6 to 9 are assumed to be Indian and get `+91`. Change the `num()` function in `index.html` if you need another country.

**Important:** a web page cannot place a call or send a message silently. Horizon opens the dialer or the app, and you tap **Call** or **Send**. Fully automatic calling needs a native Android app.

## Configuration

| Setting | Where | Default |
| --- | --- | --- |
| Claude model | `MODEL` constant in `index.html` | `claude-sonnet-5-5` |
| DeepSeek model | Model box in the app | `deepseek-flash` |
| Proxy URL | Proxy URL box in the app | none |
| Language | EN / हिं buttons | English |
| Theme | Colored dots | Gold |

Everything is saved in your browser's `localStorage`.

## How it works

1. **Listen.** The browser's Web Speech API turns your voice into text (`hi-IN` for Hindi).
2. **Think.** The text goes to Claude or DeepSeek along with the tools you have allowed.
3. **Act.** If the model calls a tool, Horizon builds an app link, dialer link, or message link and shows a button.
4. **Speak.** The reply is read aloud with the browser's speech synthesis, and each word pulses the planet.

Hand tracking uses MediaPipe's Hand Landmarker in the browser. Palm size in the camera frame gives the distance, which drives the zoom.

## Customize

- **Add an app:** add an entry to the `APPS` object in `index.html`.
- **Add a tool:** add it to the `TOOLS` array and handle it in `toolRun()`. If it needs a permission, list it in `NEEDS`.
- **Add a theme:** add a name and hue to the `THEMES` object.
- **Add a language:** extend the `HI` dictionary pattern and `rlang()`.

## Privacy and security

- Your API key is stored in your browser and sent only to the AI provider you chose (or to your own proxy). Never put your key in the file, and never commit it to GitHub.
- Anyone who can open your browser profile can read the saved key. Do not use Horizon on shared devices.
- Set a spending limit on your API account.
- Camera video stays on your device. Hand tracking runs locally after the model downloads.
- Speech recognition is done by your browser's speech service, which may send audio to its provider (Chrome uses Google's).
- Your messages and tool results are sent to Claude or DeepSeek.

## Troubleshooting

| Problem | Likely cause and fix |
| --- | --- |
| Mic blocked | The page is not on `https` or `localhost`, or permission was denied. Open it from a secure address and allow the mic. |
| No speech recognition | Use Chrome, Edge, or Safari. Firefox lacks it and Brave blocks it. You can always type commands. |
| Horizon does not reply | Check that the key is saved for the selected provider and that you are online. The feed shows the exact error. |
| "Can't reach DeepSeek" | Possible browser block (CORS). Use the Cloudflare Worker proxy. |
| Camera or hand tracking fails | Allow the camera and stay online for the first load, so the hand model can download. |
| One hand acts backwards | Tap **Swap left/right**. This only matters when one hand is visible. |
| No Hindi voice | Install a Hindi voice in Android Settings → Text-to-speech. |
| App does not o
