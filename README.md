<div align="center">

# 🕵️ TEAM LEGACY // OPERATION BLACK CHAMBER

### A steampunk stealth-puzzle game where the real cipher tools are the gameplay.

![Levels](https://img.shields.io/badge/levels-10-c59b27?style=for-the-badge)
![Single file](https://img.shields.io/badge/single-HTML%20file-b86230?style=for-the-badge)
![No dependencies](https://img.shields.io/badge/build%20step-none-33ff66?style=for-the-badge)
![Companion](https://img.shields.io/badge/works%20with-OmniTool%20V3-ffaa22?style=for-the-badge)

**[▶ Play the game](#-play-it)** · **[🧰 Get OmniTool V3](https://2ndchances520.github.io/OmniToolV3/)** · **[🗺 The 10 sectors](#-the-10-sectors)**

<!-- Add a screenshot or GIF here:
![Gameplay](docs/screenshot.png)
-->

</div>

---

## 📜 The Story

Someone inside the **Black Chamber** is selling secrets. You are **Operative Legacy**: fedora, dark glasses, magnifying glass. Sneak past patrolling sentries, crack open safes in ten sectors, and carry the intercepted data back to your cyber deck.

The catch is that the game never decodes anything for you. Every intercept is real encoded data, and you have to break it yourself using [**TeamLegacy OmniTool V3**](https://2ndchances520.github.io/OmniToolV3/) (and a historically accurate Enigma simulator). Type the decoded answer into the deck to unlock the next sector.

Nothing is faked. If you can decode it in OmniTool, you can win.

---

## 🎮 Play It

**Online:** open the GitHub Pages link for this repo (see [Hosting](#-hosting-on-github-pages) below).

**Locally:** download `SpySchool.html` and open it in any modern browser. No install, no build step.

> Keep **OmniTool V3** open in a second tab. The game has a button in the header that opens it.

---

## 🕹 Controls

| Key | Action |
|---|---|
| `W` `A` `S` `D` or Arrow keys | Sneak and move |
| `E` or `Space` | Examine the safe with your magnifying glass |

**Stealth rules:** each sentry has a detection radius. Stay inside it too long and you're caught and sent back to the sector entrance. Time your movement around their patrol routes.

**Deck buttons**

- **Copy Intel for OmniTool:** copies the intercepted data to your clipboard (unlocked once you've examined the safe).
- **Show Intel Plate (QR):** on QR sectors, displays a scannable plate you can save as a PNG.
- **Decrypt:** submit your decoded answer. Case, spaces and punctuation are ignored.

---

## 🗺 The 10 Sectors

No spoilers: this table shows which tool each sector needs, not the answers.

| # | Operation | What you'll do |
|---|---|---|
| I | Dead Drop | Reveal a message hidden in plain-looking text (**Steganography**) |
| II | Iron Telegraph | Decode a credential (**Base64 Vault**) |
| III | Sky Leviathan | Decode military-style text (**Ascii85 Vault**) |
| IV | Clockwork Enigma | Decrypt real wartime-style traffic (**Enigma Simulator**) |
| V | Ghost Eye | Save and scan a security plate (**QR Scanner**) |
| VI | Chimera | Two-stage cipher (**Steganography → Base64**) |
| VII | Ghost Negative | Decode Base64 into an *image file* and read it (**Base64 file decode**) |
| VIII | Nesting Dolls | Peel two layers (**Ascii85 → Base64**) |
| IX | Plate and Payload | Scan, then decode (**QR → Base64**) |
| X | Black Chamber | Four layers deep (**Steganography → Ascii85 → Base64 → Enigma**) |

The finale's Enigma settings depend on codes you recovered earlier, so keep an eye on the **Recovered Codes** log in the deck.

---

## 🧰 Tools You'll Use

| Tool | Where |
|---|---|
| Steganography (zero-width text) | OmniTool V3 → *Steganography* |
| Base64 text and files | OmniTool V3 → *Base64 Vault* |
| Ascii85 | OmniTool V3 → *Ascii85 Vault* |
| QR scanning | OmniTool V3 → *QR Suite V2* → *QR Scanner / Decoder* |
| Enigma | [Enigma Simulator](https://people.physik.hu-berlin.de/~palloks/js/enigma/enigma-u_v20_en.html) (linked in the game header and in OmniTool) |

Stuck? Every sector has a built-in hint that appears when you submit a wrong answer.

---

## 🌐 Hosting on GitHub Pages

1. Put `SpySchool.html` in your repo (renaming it to `index.html` gives the cleanest URL).
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then pick your branch and `/ (root)`.
4. Wait a minute, then visit `https://<your-username>.github.io/<repo-name>/`.

---

## ⚙️ Under the Hood

- **One file.** HTML, CSS and JavaScript all live in `SpySchool.html`.
- **Canvas rendering.** The detective, sentries, safes and portrait are drawn procedurally with the 2D canvas API. There are no image assets.
- **Synthesized audio.** All sound effects come from the Web Audio API. No audio files.
- **Real ciphers.** Zero-width steganography, Base64, Ascii85 and Enigma I intercepts are generated or precomputed to match what the tools expect. The sector 7 image is drawn at runtime and encoded to Base64 in your browser.
- **Progress saves automatically** to `localStorage`. Use the **Reset** button to start over.
- **One external script:** [`qrcode-generator@1.4.4`](https://cdnjs.cloudflare.com/ajax/libs/qrcode-generator/1.4.4/qrcode.min.js) from cdnjs, used only to draw QR plates. Without it (offline), QR sectors show the raw data instead.

### Adding your own sector

Missions are plain data in the `MISSIONS` array. Copy an existing entry and edit it:

```js
{
  title: "OPERATION YOUR-NAME",
  sector: "SECTOR 11: SOMEWHERE COOL",
  cipherType: "OMNITOOL // BASE64 VAULT",
  safePos: { x: 600, y: 380 },       // where the safe is
  playerStart: { x: 70, y: 80 },     // where you spawn
  guards: [ /* patrol routes */ ],
  rawIntercept: "...",               // what gets copied to the clipboard
  passcode: "THE_ANSWER",            // what the player must decode
  desc: "...", hint: "...", story: "..."
}
```

Set `kind: "qr"` to display the intercept as a QR plate, or use `stego: { cover, secret }` to hide the secret in zero-width characters automatically.

---

## 🛠 Changelog

**v2: Ten sectors**
- Added sectors VII to X with new mechanics (image decode, layered ciphers, QR chains, four-layer finale)
- Added a story beat after every sector and a Recovered Codes log
- Fixed the Sector III syntax error caused by unescaped quotes
- Fixed Sector IV: Enigma can't encrypt digits, so the answer is now letters only, with a correctly computed ciphertext
- Sector V now shows a real scannable QR plate
- Answer checking ignores case, spaces and punctuation
- Old 6-sector save files still load

---

## 🔭 Ideas for Part 2

- A sector using OmniTool's **Voice Synthesis** and **Transcription** (a spoken passphrase to transcribe)
- A **Time Capsule** sector that unlocks on a real date
- Mobile touch controls
- Sentry vision cones instead of circles

---

## 🤝 Credits

Built by **Team Legacy**. Companion toolkit: [OmniTool V3](https://2ndchances520.github.io/OmniToolV3/). Enigma simulator by Frank Palloks, hosted at Humboldt-Universität zu Berlin.

<div align="center">

*Good luck, Operative. The Chamber is listening.* 🔐

</div>
