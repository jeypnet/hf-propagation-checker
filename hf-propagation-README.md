# hf-propagation-checker

**Real-time HF band propagation checker using live NOAA Space Weather data**

Fetches solar flux (SFI), planetary K-index (Kp), and X-ray flare data directly from NOAA SWPC APIs and calculates estimated propagation conditions for every HF amateur band from 160m through 10m.

No paid APIs. No registration. Pure Python. Live data.

---

## Background

Built by an Extra Class Ham Radio operator with field experience selling and supporting software-defined radio systems at [FlexRadio Systems](https://flexradio.com). A common question from FlexRadio customers — especially in defense, utilities, and emergency response — was *"is the band open right now?"*

This tool answers that question programmatically, using the same NOAA data professional operators rely on.

---

## Sample Output

```
════════════════════════════════════════════════════════════
  HF PROPAGATION REPORT — 2026-03-03 14:30Z
════════════════════════════════════════════════════════════
  Solar Flux Index (SFI) : 145
  Planetary K-index (Kp) : 2.1
  X-Ray Flare Class      : None
════════════════════════════════════════════════════════════
  BAND         FREQ   SCORE  RATING
────────────────────────────────────────────────────────────
  160m        1.8 MHz  100.0  EXCELLENT 🟢
  80m         3.5 MHz  100.0  EXCELLENT 🟢
  40m         7.0 MHz  100.0  EXCELLENT 🟢
  30m        10.1 MHz  100.0  EXCELLENT 🟢
  20m        14.0 MHz  100.0  EXCELLENT 🟢
  17m        18.1 MHz  100.0  EXCELLENT 🟢
  15m        21.0 MHz  100.0  EXCELLENT 🟢
  12m        24.9 MHz  100.0  EXCELLENT 🟢
  10m        28.0 MHz   92.5  EXCELLENT 🟢
────────────────────────────────────────────────────────────

  NOTES:
  [10m] SFI 145 below 150 minimum for 10m
════════════════════════════════════════════════════════════
  Data: NOAA Space Weather Prediction Center (swpc.noaa.gov)
════════════════════════════════════════════════════════════
```

---

## Quickstart

```bash
git clone https://github.com/jeypnet/hf-propagation-checker.git
cd hf-propagation-checker
python hf_propagation.py
```

No dependencies — uses Python standard library only.

---

## Usage

**Full report (live NOAA data):**
```bash
python hf_propagation.py
```

**Check a single band:**
```bash
python hf_propagation.py --band 20m
```

**Export results to JSON:**
```bash
python hf_propagation.py --json output.json
```

**Test with custom values (no API call):**
```bash
python hf_propagation.py --sfi 180 --kp 4.5
```

---

## How It Works

The script pulls three real-time data points from NOAA SWPC:

| Parameter | What It Measures | Why It Matters |
|---|---|---|
| **SFI** (Solar Flux Index) | Solar radiation at 10.7cm | Higher = better F-layer ionization = higher bands open |
| **Kp** (Planetary K-index) | Geomagnetic disturbance 0–9 | Higher = more HF disruption, especially on lower bands |
| **X-ray class** | Solar flare intensity (A/B/C/M/X) | M/X class = D-layer absorption = HF blackout on sunlit side |

Each band is scored 0–100 based on its minimum SFI requirement, sensitivity to geomagnetic disturbance, and current flare activity. Ratings: **EXCELLENT / GOOD / FAIR / POOR / CLOSED**

---

## Data Sources

All data fetched live from NOAA Space Weather Prediction Center:

- Solar Flux: `services.swpc.noaa.gov/products/summary/10cm-flux.json`
- K-index: `services.swpc.noaa.gov/products/summary/planetary-k-index.json`
- X-ray flares: `services.swpc.noaa.gov/products/summary/xray-flares.json`

Free, public, no API key required.

---

## Requirements

- Python 3.10+
- No external dependencies — standard library only (`urllib`, `json`, `datetime`)

---

## Field Context

During my time at FlexRadio, customers running HF communications for emergency response and utility operations frequently needed to know band conditions before deploying. This tool automates what experienced operators check manually — solar indices, geomagnetic stability, and flare activity — and translates them into actionable band-by-band ratings.

For integration with FlexRadio hardware, see the companion repo:
👉 [flexradio-smartsdr-client](https://github.com/jeypnet/flexradio-smartsdr-client)

---

## License

MIT

---

*Part of the [jeypnet](https://github.com/jeypnet) project portfolio.*
*Extra Class Ham Radio Operator | FlexRadio Systems alumnus*
