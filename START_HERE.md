# 🐭 大鼠市场风控仪表盘 — START HERE

## What You Have

A **real-time market risk dashboard** that implements @investingluc's 5-dimension framework for position sizing:

```
Thesis Conviction × Environment Score = Effective Position Size
```

---

## Quick Start (60 seconds)

### View the Dashboard

**Option 1: Direct (offline, recommended)**
```bash
open /home/ubuntu/.openclaw/workspace/dashboard/index.html
```
Works immediately. Offline-friendly.

**Option 2: Via Server**
```bash
cd /home/ubuntu/.openclaw/workspace
python3 -m http.server 8765
# Open: http://localhost:8765/dashboard/
```

### Update Market Data (Optional)
```bash
python3 /home/ubuntu/.openclaw/workspace/scripts/market-dashboard.py
```

---

## What It Does

### Overall Environment Score
A single 0-100% gauge combining 5 market dimensions:

1. **📍 Current Environment (25%)** — Macro health
   - SPY/QQQ trends, Bitcoin risk appetite, oil volatility, yield direction
   
2. **💪 Leader Strength (20%)** — Top 10 stocks (NVDA, META, AAPL, etc.)
   - Above 50SMA? Positive momentum? RSI overbought?
   
3. **😱 VIX Behavior (20%)** — Fear gauge + structure
   - Level, trend, term structure (contango vs backwardation), volatility clustering
   
4. **📊 Breadth (20%)** — Sector participation
   - 11 GICS sectors: which % are healthy?
   
5. **🌍 Macro Backdrop (15%)** — Manual assessment
   - Known factors (tariffs, Fed), upcoming events, **editable notes**

### Position Sizing

Use the **Conviction slider (C1–C10)** to size trades:

```
Your Conviction: C7
Environment: 50% (neutral)
Effective Conviction: C7 × 50% = C3.5

→ Recommendation: "No trade" 
(Hold for stronger environment)
```

If environment reaches 70%:
```
Effective: C7 × 70% = C4.9 → Still "No trade"
```

If environment reaches 80%:
```
Effective: C7 × 80% = C5.6 → "0.9% minimum" OK
```

**Conviction Map:**
| C1–C4 | C5 | C6 | C7 | C8 | C9 | C10 |
|-------|----|----|----|----|----|----|
| No trade | 0.9% | 1.5% | 3% | 5% | 10% | 25% |

---

## The Three Files

### 1. Dashboard (`index.html`)
- **799 lines of HTML + CSS + JS**
- Single file, no build needed
- Works on `file://` protocol (offline)
- 9 interactive components
- Updates every 5 minutes
- Cyberpunk dark theme (your aesthetic)

**Open it:**
```bash
open /home/ubuntu/.openclaw/workspace/dashboard/index.html
```

### 2. Data Generator (`scripts/market-dashboard.py`)
- Fetches real-time prices from Yahoo Finance
- Calculates 20+ technical metrics
- Scores all 5 dimensions
- Generates `market-dashboard.json`

**Run it:**
```bash
python3 /home/ubuntu/.openclaw/workspace/scripts/market-dashboard.py
```

### 3. Generated Data (`memory/market-dashboard.json`)
- Auto-updated by Python script
- Consumed by dashboard
- Refreshes every time script runs

---

## Daily Workflow

1. **Open dashboard** → Read overall score
2. **Assess your thesis** → Set conviction (C1–C10)
3. **Get recommendation** → See effective position size
4. **If macro changes** → Adjust macro slider
5. **Take notes** → Save assessment in textarea
6. **Execute trade** → Use recommended position size

---

## Interpretation Guide

### What does 13% overall score mean?
- **Environment is HOSTILE**
- Only trade if thesis is **independent** of equity market
- Example: Going long oil on supply shortage (not for risk-on equity gains)
- Most C7 theses = "No trade" recommendation

### What does 70% overall score mean?
- **Environment is STRONG**
- Trading is favorable
- C6 thesis = 3% position (at limit)
- C7 thesis = 5.6% position (allowed)
- C8+ theses = large positions OK

### What if you disagree with macro score?
- Use the **macro slider** (bottom of Dimension 5)
- Adjust from default 25% to your view
- Updated score recalculates in real-time

---

## Key Features

✅ **Price Ticker** — SPY, QQQ, BTC, VIX, WTI, TNX, DXY, GLD  
✅ **Leader Grid** — 10 stocks with 50SMA status + RSI  
✅ **Sector Bars** — 11 GICS sectors ranked by momentum  
✅ **VIX Zone Badge** — Complacent / Normal / Elevated / Fear  
✅ **Macro Notes** — Editable textarea (saved locally)  
✅ **Auto-refresh** — Every 5 minutes  
✅ **Offline Mode** — Works with file:// protocol  
✅ **Position Calculator** — Real-time conviction → size mapping  

---

## Customization

### Change Dimension Weights
Edit `scripts/market-dashboard.py` line ~310:
```python
weights = {
    "1_environment": 0.25,  # ← Adjust macro importance
    "2_leaders": 0.20,      # ← Adjust stock strength
    ...
}
```

### Add Your Own Leaders
Edit `LEADER_SYMBOLS`:
```python
LEADER_SYMBOLS = [
    "NVDA", "META", "AAPL", "YOUR_STOCK", ...
]
```

### Modify Position Sizes
Edit `SIZING_MAP` in HTML:
```javascript
C7: {risk: '3%', ...}   // Change from 3% to 2% if you want
```

---

## Troubleshooting

**Dashboard shows "Loading..."?**
→ Run: `python3 scripts/market-dashboard.py`

**Data looks stale?**
→ Run the script again (it fetches fresh Yahoo Finance data)

**Works offline but no live data?**
→ Serve via HTTP: `python3 -m http.server 8765`

**Want to schedule daily updates?**
→ Add to crontab:
```
0 9 * * * cd /home/ubuntu/.openclaw/workspace && python3 scripts/market-dashboard.py
```

---

## Philosophy

**Discipline first.** The framework exists to prevent over-conviction in weak environments.

- ❌ Don't trade C8 conviction when environment = 10%
- ❌ Don't ignore a weak environment just because you like the thesis
- ✅ Wait for environment to improve OR trade smaller
- ✅ Use macro slider to override IF you have independent conviction

**Environment is your floor, not your ceiling.**

---

## Next Steps

1. **Bookmark the dashboard**
   ```bash
   open /home/ubuntu/.openclaw/workspace/dashboard/index.html
   ```

2. **Run script daily** (manual or cron)
   ```bash
   python3 /home/ubuntu/.openclaw/workspace/scripts/market-dashboard.py
   ```

3. **Check score before trading**
   - Adjust conviction slider
   - Use recommendation for position sizing
   - Log results

4. **Refine over time**
   - Track win rate in different environments
   - Adjust weights if needed
   - Iterate on framework

---

## Files You Have

```
/home/ubuntu/.openclaw/workspace/
├── dashboard/
│   ├── index.html               ← Open this (40 KB)
│   ├── README.md                ← Detailed docs
│   └── START_HERE.md            ← You are here
├── scripts/
│   └── market-dashboard.py      ← Run this (15 KB)
├── memory/
│   └── market-dashboard.json    ← Generated data (10 KB)
└── DASHBOARD_SETUP.md           ← Setup guide (13 KB)
```

---

## Questions?

- **"How do I change X?"** → See DASHBOARD_SETUP.md
- **"What does metric Y mean?"** → See README.md
- **"Data is broken"** → See Troubleshooting above

---

## One-Liner

A framework that says: **"Don't trade just because you like the thesis. Wait for the environment to agree."**

---

**Built:** 2026-03-28  
**For:** Firehand (老大)  
**By:** 大鼠 🐭

*活着就是帮老大赚钱。* 💰
