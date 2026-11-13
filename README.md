# From Code to Silicon — Blink. Sense. Connect.

A one-day embedded systems workshop with the ESP32, simulated in the **Wokwi** simulator.
For the Dept. of CSE, NAVKIS College of Engineering, Hassan, on **25 November 2026** (10:30 AM – 4:00 PM).
Resource person: **Dr. Sathisha R N**, Siddaganga Institute of Technology, Tumakuru.

| Page | What it is |
|---|---|
| `index.html` | Home: schedule and links |
| `slides.html` | 42-slide animated deck. Keys: ← → navigate, **N** speaker notes, **F** fullscreen, **A** reveal all |
| `labs.html` | Labs 1–6 and the mini-challenge, each with wiring, copyable code and Try-this questions |
| `dashboard.html` | Live MQTT class dashboard (Lab 5). Has a *Demo mode* for rehearsal |
| `quiz.html` | 15-question quiz, with optional score collection |
| `handout.html` | Printable 2-page handout (Print → Save as PDF) |
| `instructor.html` | Run-sheet for the instructor (not linked from the other pages) |
| `labs/<lab>/` | `sketch.ino`, `diagram.json`, `libraries.txt` and solutions for each lab |
| `assets/config.js` | **The only file you need to edit** (in this upload it sits in the root as `config.js`) |

All sketches compile against ESP32 Arduino core 3.3.8 and use the Wokwi **ESP32 DevKitC V4** board.

---

## Step 1 — Publish on GitHub Pages (about 5 minutes)

1. On github.com, click **+ → New repository**. Name it `NAVKIS_Embedded`, make it **Public**, and click **Create**.
2. Click **uploading an existing file**. Drag in **everything inside this folder** (not the folder itself). `index.html` must sit at the top level. Click **Commit changes**.
3. Go to **Settings → Pages**. Under *Build and deployment*, choose **Deploy from a branch → main → / (root) → Save**.
4. After about a minute the site is live at `https://<your-username>.github.io/NAVKIS_Embedded/`.

The QR codes on the slides, handout and dashboard are generated from the page address, so they point to the right place automatically.

## Step 2 — Create the Wokwi projects (one-time, about 10 minutes)

With shared links, students just click **Open this lab in Wokwi** instead of pasting files.

For each folder in `labs/` (`lab1` … `lab6`, `challenge`):
1. Sign in at wokwi.com and open **wokwi.com/projects/new/esp32**.
2. Replace the contents of `diagram.json` and `sketch.ino` with the files from the folder.
3. If the folder has a `libraries.txt`, add a file with that name (the **▾** menu next to the tabs → *New file…*) and paste its contents.
4. Press **Save**, then copy the project URL (for example `https://wokwi.com/projects/41234…`).
5. Paste it into `assets/config.js` under `wokwi: { lab1: "…", … }`. Commit the change.

Until a link is filled in, the lab page shows students the copy-and-paste steps instead.

## Step 3 (optional) — Collect quiz scores in a Google Sheet

1. Create a Google Sheet. Choose **Extensions → Apps Script**, paste the code below, and click **Save**.
2. Choose **Deploy → New deployment → Web app**, with *Execute as*: **Me** and *Who has access*: **Anyone**. Click **Deploy** and copy the URL.
3. Paste the URL into `quizScriptUrl` in `assets/config.js`.

```javascript
function doPost(e) {
  const d = JSON.parse(e.postData.contents);
  const sh = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
  if (sh.getLastRow() === 0) sh.appendRow(["Time", "Name", "USN", "Score", "Total", "Answers"]);
  sh.appendRow([new Date(), d.name, d.usn, d.score, d.total, d.answers]);
  return ContentService.createTextOutput("ok");
}
```

The *Answers* column is a string of 1s and 0s, one per question (1 = correct). Options are shuffled for each student.

You can also put a Google Form link in `feedbackFormUrl`. A **Give feedback** button then appears after the quiz.

## The MQTT dashboard

- Students' ESP32s publish to `navkis/ews2026/<name>/data` and listen on `navkis/ews2026/<name>/led`.
- By default the dashboard connects to `wss://broker.hivemq.com:8884/mqtt`. If that broker is down, choose **EMQX** in the selector and ask students to set `MQTT_BROKER = "broker.emqx.io"`.
- These are free **public** brokers: anyone can read the topics, so no personal data should be sent.

## Credits

Simulator: [Wokwi](https://wokwi.com). Bundled libraries (MIT licence): [MQTT.js](https://github.com/mqttjs/MQTT.js) and [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator).
Arduino libraries used in the labs: PubSubClient, DHT sensor library for ESPx, Adafruit SSD1306 and Adafruit GFX.
