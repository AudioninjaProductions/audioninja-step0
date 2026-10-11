# AudioNinja Step 0 Calculator

**Current version: 1.2.0**

A single-file, browser-based prep tool from Audio Ninja Productions. Enter what you know about a venue and a speaker rig, and it estimates delays, levels and coverage so you arrive at live calibration with targets.

Step 0 is prep only. It never produces DSP values. Nothing here goes into the system until live calibration is signed off.

## What it does

- **Delay estimates** per cluster (FOH L/R, Front Fill, Sub, Delay Fill) using the corrected speed of sound, c = 331 + 0.6 x temperature (C).
- **Level estimates** per cluster and combined Mains level, with level at the furthest audience point against your target SPL.
- **Coverage check** from horizontal and vertical angles, trim height and FOH spread.
- **Flags** for problems such as coverage gaps, reflective rooms, and sub-to-top level differences.
- **Defaults applied** list: anything left blank uses an industry-standard assumption, and the app tells you which.
- **CalTemp**: a step-by-step live calibration workflow built from your result (SPL and headroom mapping, time and phase alignment, live transfer function tuning, playback and coverage verification).
- **Lookup**: fills venue fields, and each speaker cluster (FOH-L/R tops, Center, Subs, Delay) has its own model box and look-up button that fills that cluster's RMS watts, sensitivity, peak SPL and coverage angles. Everything it fills is marked as unverified, and anything it cannot find stays blank for you to type.
- **Save this model (v1.4.0)**: each cluster has a button that stores the make, model and the specs entered there on the device. Saved models are used first in lookups, are marked unverified, and can be exported with **Copy my models** / **Download my models** so a maintainer can check them against the data sheet and add them to `models.json`.
- **Speaker model table (v1.3.0)**: 74 models from manufacturer data sheets (Mackie, JBL, QSC, Yamaha, RCF, Peavey, Electro-Voice, d&b, L-Acoustics, Meyer, Martin Audio, Nexo). Press a cluster's lookup button with a known model and it fills offline, with the data sheet link. Blank means not published; derived values are flagged.
- **Brands tab** (now the Database tab): an offline reference of 315 loudspeaker and amplifier brands (speakers, amplifiers, both, and OEM drivers and amplifier modules, vintage to current) with search and filters. The speaker model boxes suggest brand names as you type. **Update database** downloads a newer list from this repository when online.
- **Sub setup**: subs are marked L/R stacked with FOH or mono center, and omni, cardioid or end-fire.
- **Help tab**, **Copy summary**, and an automatic **update check**.

All figures are free-field estimates. Real rooms, especially reflective ones, will differ. Power and electrical draw are not checked.

## Install

1. Download `AudioNinja_Step0_Calculator_Team.html` from this repository (open the file page, then use the download button).
2. Open the file in Chrome (or any browser). No install, no account, and it works offline.

Your inputs and settings are saved in your own browser on your own computer.

## How to use

1. **Fill in what you know.** Open the Inputs side. Leave unknowns blank. Enter the venue (room size, ceiling, wall materials), temperature at the mix position, target SPL, furthest audience distance, and each speaker cluster (quantity, which starts at 1, sensitivity and watts or rated peak SPL, distance from the Mains reference). A Center, Sub or Delay cluster with nothing entered but the quantity is left out of the rig. For Subs, also set the configuration and pattern. Use **Load example** to see a worked rig and **Clear all** to start blank.
2. **Optional: Lookup.** Type a venue name and city and tap **Ask AI to look up the venue**. For speakers, type a model inside a cluster and tap **Ask AI about this speaker** in that cluster. Filled fields get a blue edge and are unverified until you confirm them. Use **Undo import** to restore your earlier values.
3. **Read the Results.** Check the delay table, levels, flags and defaults. Use **Copy summary** to paste into notes or messages.
4. **Open CalTemp** for the live calibration sequence. Do not EQ before SPL and time alignment are confirmed, and never boost a null.

## AI lookups (no key needed)

The app no longer asks for an AI key and never calls an AI itself. **Ask AI** works like this:

1. If the speaker model is in the built-in table, the cluster fills instantly and offline from the manufacturer data sheet.
2. Otherwise it copies a ready-made request and opens your AI chat (Claude, ChatGPT or Gemini, chosen under **AI chat to open for lookups**).
3. Send the request, copy the AI's whole reply, return to the app and press **Paste AI reply and fill**. If the browser blocks clipboard reading, paste into **Import lookup data** and press **Fill fields**.

Speaker specs can also be read from pasted text or a loaded text file in each cluster (**Read specs**), with no AI.

## Updates

When your computer is online, the app checks this repository's `version.json` and shows a banner if a newer version exists. A web page cannot replace its own file, so download the new file and open it in place of the old one. The Help tab has a **Check for updates now** button.

## Speaker models database

`models.json` holds one row per model with the values copied from the manufacturer data sheet and the URL it came from. Blank = not published or ambiguous. Maintainers: add rows, raise `dbVersion`, commit. **Update database** pulls `models.json` from the same folder as `brands.json`. Fields: b brand, m model, t top|sub|full, p powered Y|N, w watts, wl rms|aes|cont|prog, wpk peak watts, sens dB, spl max SPL, splt peak|unspec, splc continuous SPL, h/v degrees, pat omni|cardioid|endfire, fr, wt kg, url, note. Not exhaustive: Bose, Behringer and Turbosound sites blocked automated reading.

## Brands database

The list is built into the app, so it works offline. When online, **Update database** (Brands tab) downloads `brands.json` from this repository and keeps it in the browser if it is newer. It is a reference list, not a spec sheet. It is not exhaustive, was compiled from general knowledge, and ownership and status change over time. A blank status means unknown.

To update it: edit `brands.json` on GitHub, add brands in the same shape (`n` name, `k` type: speaker, amp, both or oem, `s` segments, `c` country, `st` active, legacy or blank, `note`), and raise `dbVersion` (for example `2026.11.02`). Users get it the next time they press Update database.

## For the maintainer: publishing an update

1. Upload the new HTML file with the same name, `AudioNinja_Step0_Calculator_Team.html`.
2. Edit `version.json`: raise `version` and update `notes`.
3. GitHub may take about 5 minutes to serve the new `version.json`.

## Version history

- **1.12.0** Delay approach selector. Ideal design places and tests delays at the 6 dB drop point (one doubling of distance). Budget uses the 10 dB drop from the Scrolls' Step 4 fill threshold, for venues that cannot pay for delay clusters. Only sets the default when the Delay L distance is blank.
- **1.11.1** Peer-review checks: warning if the front row distance or a typed delay distance is beyond the furthest audience point.
- **1.11.0** Application criteria: Full range music (DJ, band with drums and bass) uses every cluster; Acoustic (no drums or bass) and Speech only leave the subs out of the delay, level and CalTemp estimate (typed sub values are kept). Delay and Center prediction apply to all three. Saved with the form and in the summary.
- **1.10.0** Center cluster auto-off. With the Center distance blank, the app finds where the Mains L and R patterns meet, (spread / 2) / tan(horizontal angle / 2). If that is at or inside the front row distance (new input, blank = 2 m), the Center is left out of the estimate. Otherwise it is placed at the Mains line (0 m). Assumes the coverage angle is the -6 dB edge, where two edges sum to about -3 dB. Geometry extension, not in the Scrolls.
- **1.9.2** Auto-off: with the Delay L distance blank, if the 6 dB default point is beyond the furthest audience point the delays are left out of the estimate (marked Off in the table, with a note). Typed quantities are kept; entering a distance brings them back.
- **1.9.1** Default delay distance: leave the Delay L distance blank and it is set to where the Mains-only level (inverse square) falls 6 dB below the target SPL, listed under Defaults applied. A flag appears if that is beyond the furthest audience point.
- **1.9.0** Delay is now Delay L and Delay R, like the FOH pair. Delay R follows Delay L by default (same speaker, quantity and distance, one timing for the pair); untick "Same as Delay L" to enter it separately. Imports accept "delay_right", "fill_r" and similar names; a single delay entry fills both sides.
- **1.8.0** Alignment reference: Auto picks the cluster farthest from the listening position as the zero point (Mains, Subs or Center can also be chosen). All delays, the results table, CalTemp passes and the summary follow it. Mains ahead of the subs now gives the Mains the delay.
- **1.7.1** Clear all also clears the sound system box, per-cluster pasted text and files, the import box, lookup markers and undo.
- **1.7.0** AI settings and API keys removed. Ask AI copies the request and opens the chosen AI chat, Paste AI reply and fill brings the reply back. Copy request, Read with AI and Extract with AI buttons removed.
- **1.6.0** Per-cluster spec paste/file box: paste text from a search result or load .txt/.csv/.md/.json/.html and read watts, sensitivity, max SPL, coverage and sub pattern offline, or with the AI. PDFs must be copy-pasted.
- **1.5.1** Sound system box fills clusters offline from the model table (button + applied after every AI import); importer finds the speaker list under any heading and with models only.
- **1.5.0** Venue and sound system in one request and one import: sound system box, combined prompt, Fill fields populates the room and all clusters (quantity, sub setup and pattern). Table models use data sheet specs.
- **1.4.3** Ambiguous model names list the matches instead of doing nothing; clear message when a model is not in the table; 6 more Mackie Thump models (74 total, models.json 2026.10.12).
- **1.4.2** Fix: Import lookup data / Fill fields did not fill when the AI wrote numbers as text, in feet, or in nested groups. Now handled, with a clearer message when nothing matches.
- **1.4.1** Copy request for AI chat button beside each lookup button (venue and every cluster).
- **1.4.0** Save this model to the list, per cluster, with copy/download of saved models.
- **1.3.0** Speaker model spec table with offline autofill per cluster, models in Update database, Database tab, Help section on models.
- **1.2.0** New Brands tab with the offline brand database, search and filters, brand suggestions on the speaker model boxes, and an Update database button. Phone tab row scrolls sideways instead of clipping.
- **1.1.0** Each cluster has its own speaker lookup. Quantity defaults to 1 and a line under Clusters shows how far the Mains are from the target. Subs have a configuration (L/R stacked with FOH, or mono center) and a pattern (omni, cardioid, end-fire). Front Fill is now called Center. Coverage angles live on the cluster that owns them (FOH tops drive the coverage check). Sub configuration and pattern do not change the level math; the cardioid and end-fire alignment note is general practice and not from AudioNinjaScrolls.
- **1.0.3** CalTemp Step 2 now starts with the Reaper calibration template build (Tones, RTA and Time Alignment folders; Fill L and Fill R are the delay clusters), taken from the Sound System Calibration book and the template file.
- **1.0.2** CalTemp Step 2 now lists the full waveform-matching procedure from Ninja Calibration (DAW setup, the two 10-second recordings per cluster, nudging the leading edge, noting the offset, confirming against the FFT before writing to DSP) and has a table to enter each recorded offset and compare it with the Step 0 estimate.
- **1.0.1** Stress-test fixes: out-of-range or absurd entries are flagged and ignored instead of breaking results or the chart; phone layout no longer scrolls sideways; AI and update calls time out instead of hanging.
- **1.0.0** First release: input-to-results calculator, CalTemp, venue and speaker Lookup (Gemini, Claude, or copy and paste), Help tab, update check, offline use.

## Notes

- Formulas follow AudioNinjaScrolls. The coverage reach geometry is an extension of the Scrolls, not a Scrolls formula.
- If a result looks wrong, or a field behaves differently from how you work, flag it so AudioNinjaScrolls can be updated.

(c) Audio Ninja Productions
