# AttendIQ — Smart College Attendance Tracker

A single-file attendance tracker for college students. Log every class with one click, see instantly whether you're meeting your 75% target, and let AI read your college timetable screenshot to set up all your courses at once.

**No installation. No backend. No signup.** Open `index.html` in any browser and start tracking.

---

## Features

- **One-click logging** — mark a class Attended, Missed or Cancelled from the course card; percentages, buffers and advisories update instantly.
- **AI timetable scanner** — drag & drop a schedule screenshot; Google Gemini extracts each course and its weekly count automatically (with automatic fallback through substitute models if one is overloaded), then **a second AI pass recounts the grid** and every number is shown in a review step for you to confirm or correct — nothing replaces your list until you approve it.
- **Bunk & catch-up math** — tells you how many classes you can skip safely, how many in a row you must attend to recover, and how much semester buffer you have left.
- **Target-based warnings** — every course is colour-coded against your target percentage (75% by default, configurable).
- **Backup & restore** — export all data as JSON and import it back on another device or browser.
- **Private by design** — all data lives in your browser's local storage. Nothing is sent anywhere except the timetable image you explicitly scan.

## Quick start

1. **Download** `index.html` (or clone this repo) and open it in Chrome, Edge or Firefox.
2. *(Optional, for the AI scanner)* Get a free Gemini API key from [aistudio.google.com/apikey](https://aistudio.google.com/apikey) and paste it into **Settings → Gemini API key → Save settings**.
3. Use **Add subject** to enter courses manually, or use the **Timetable scanner** to read them from a screenshot. Done — start logging classes.

## 5-minute tutorial

**1 · Configure (once).** Click **Settings** in the top bar (or the Settings header in the page) and set:
- *Gemini API key* — only needed for the AI scanner.
- *Total weeks in semester* — e.g. 15. Used to estimate semester totals.
- *Target attendance %* — usually 75. Everything is measured against this.
Click **Save settings**. The panel can be collapsed again — it stays tidy on every visit.

**2 · Add your courses, the fast way.** Open the **Timetable scanner**, drop a photo or screenshot of your weekly schedule and click **Extract subjects**. The AI reads every course and its weekly count — then a **second pass recounts the entire grid** to catch counting mistakes. The result opens as a **review list**: every course with an editable "classes per week" number, and a flag (amber note) wherever the two AI passes disagreed. Fix anything that still looks wrong and click **Apply timetable**.

<details>
<summary>Why the review step matters</summary>

Vision models are excellent at *reading* a timetable but occasionally mis-**count** repeated cells — say 6/week for a subject that actually appears 7 times. That's why the extraction is never trusted blindly: a second recount runs over the same image, disagreements are flagged for you, and nothing touches your saved courses until you press **Apply**. Cancelling the review discards the scan and leaves your data untouched.

</details>

**3 · Add courses manually (alternative).** **Add subject** → name, classes per week, and semester total (auto-filled as weekly × weeks). Before the semester has started, that's all you need.

**4 · Log classes daily.** On each course card:
- **Attended / Missed / Cancelled** — one click per class as your day happens.
- The minus buttons below undo a mis-click (they disable at zero).
- The big percentage, the progress bar and the tick against *target* are refreshed after every click, so you always know where you stand.

**5 · Read the advisories.**
- **Safe zone (green)** — "You can skip the next *N* consecutive classes."
- **Danger zone (red)** — "You must attend the next *N* consecutive classes without missing."
- **Critical alert (red)** — the target can no longer be reached this semester.
- **Semester buffer** — total misses you can afford for the rest of the semester; at deficit it shows the best final percentage still achievable.

**6 · Back up before the semester resets.** **Export** downloads a dated JSON with settings + all courses; **Import** restores it. Do this before clearing browser data or switching devices — the tracker has no server, so local storage *is* your database.

## The math, briefly

Your attendance is **attended ÷ (attended + missed)** — cancelled classes don't count as conducted, and the semester total adjusts for them (`expected total − cancelled`). For every course the app derives:

| Figure | Meaning |
| --- | --- |
| % | Current attendance against target |
| Safe zone | Maximum classes you may now skip and still stay ≥ target |
| Catch-up | Consecutive classes you must attend to get back to target |
| Semester buffer | Misses remaining across the whole semester before the target becomes unreachable |

## Status colours

| Colour | Meaning |
| --- | --- |
| 🟩 Green | Attended / attendance on track |
| 🟥 Red | Missed / attendance at risk |
| ⬜ Grey | Cancelled class |
| 🟨 Amber | Zero semester margin (warning) |
| 🟦 Blue | Reserved: excused status |

## Tech notes

- **One file.** `index.html` contains the markup, styling (Tailwind CDN + design tokens) and logic. No build step, no dependencies to install.
- **Storage.** Everything persists under the localStorage key `attend_iq_master_state_v2`. Clearing site data removes it — export backups regularly.
- **AI models.** The scanner tries `gemini-flash-latest`, then `gemini-2.5-flash`, `gemini-2.0-flash`, `flash-lite` variants — if one model is overloaded, the next one takes over automatically. After a successful extraction, a second model call verifies the counts, and the results open in a review step before anything is saved.
- **Privacy.** The timetable image you scan is sent to Google's Gemini API (vision) for analysis; the API key is stored in your browser only. Everything else never leaves your device.
- **Accessibility.** Keyboard-navigable controls, visible focus states, 44px touch targets on touch devices, reduced-motion support.

## FAQ

**The scanner says "high demand, try again". Should I retry?**
Just click **Extract subjects** again — the app already tries every substitute model first (for both the extraction and the verification pass); the error only appears after all of them are busy.

**I cleared my browser data and lost everything.**
Without an exported JSON backup there is nothing stored elsewhere to restore. Export early, export often.

**Excel export?**
Not yet — the JSON export keeps the data portable, and any spreadsheet tool can open or convert it.

## License

[MIT](LICENSE)
