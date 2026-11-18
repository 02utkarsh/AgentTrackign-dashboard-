# Part 3 · The Three Charts

## Constraint
If I could show only **three charts** on the AI Governance Dashboard to communicate
usage and spend-limit information to a CFO or IT/security lead — here's what I'd pick and why.

---

## Chart 1 · Spend vs. Budget Limit (Progress Bars per Tool)

**Type:** Horizontal progress bar per AI tool, color-coded by severity.

```
ChatGPT  ████████████████████▓░  92%   $9,200 / $10,000   ⚠ CRITICAL
Claude   ████████████░░░░░░░░░░  61%   $4,880 / $8,000
Copilot  █████████░░░░░░░░░░░░░  48%   $3,840 / $8,000
Midjourney ██████████████░░░░░░  78%   $3,120 / $4,000    ⚠ WARNING
Perplexity ██████████████████████ NO LIMIT — Shadow AI ❌
```

**Why this chart?**
- It answers the **single most important CFO question first**: "Are we going over budget?"
- Color encoding (green → yellow → red) lets a non-technical exec understand risk in 3 seconds.
- Surfacing **Shadow AI tools with no limit** in the same view makes the governance gap visceral.
- The "spend vs. limit" framing is immediately actionable: IT knows exactly which tool to flag.

**What it drives:**
CFO can say "cap ChatGPT at $10K" and trigger a purchase order review.
IT can block Perplexity org-wide from this screen.

---

## Chart 2 · Monthly Spend Trend (Line Chart with Budget Ceiling)

**Type:** Line chart · 6-month rolling window · Two lines: Actual Spend + Budget Ceiling (dashed red).

```
$35K  ─────────────── Budget Ceiling ─────────────── (dashed red)



$24K                                           ●  (Nov MTD)
                                          ●
$18K                               ●
                              ●
$14K    ●   ●
       Jun Jul Aug Sep Oct Nov
```

**Why this chart?**
- It answers the **second most important question**: "Are we trending toward a budget breach?"
- A CFO needs **trajectory**, not just a snapshot. If spend is linear and the ceiling is 46% away,
  that's fine. If spend is exponential and crossing the ceiling in 3 weeks — that's a crisis.
- The dashed ceiling line creates a visual "danger zone" without needing any annotation.
- Useful for board-level reporting: a single screenshot tells the AI spend story for a quarter.

**What it drives:**
Forecast alerts ("At current rate, you'll breach $35K by Nov 22").
Budget reallocation conversations with Finance before month-end.

---

## Chart 3 · Active Users by Department — Approved vs. Shadow AI (Stacked Bar)

**Type:** Horizontal stacked bar chart · Departments on Y-axis · Approved (purple) vs. Shadow AI (red) user counts.

```
Engineering  ██████████████████░░░░  98 approved  ▓ 4 shadow
Marketing    ████████████░░░░░░░░░░  72 approved  ▓ 6 shadow
Finance      ████░░░░░░░░░░░░░░░░░░  24 approved  ▓▓ 18 shadow  ← HIGH RISK
Legal        ████████░░░░░░░░░░░░░░  46 approved  ▓ 2 shadow
Product      ██████░░░░░░░░░░░░░░░░  38 approved  ▓ 7 shadow
```

**Why this chart?**
- It answers the **IT Security / CISO question**: "Where is the Shadow AI risk concentrated?"
- Finance using unapproved AI tools is a **data-exfiltration risk** (revenue data, forecasts going to
  third-party LLMs). This chart makes that visible immediately.
- The stacked format lets you see adoption breadth (total users) and risk (shadow %) simultaneously.
- Departments with high shadow usage = candidates for an approved-tool rollout or policy enforcement.

**What it drives:**
IT can prioritize which department gets next month's AI tool audit.
HR/Legal can initiate acceptable-use policy acknowledgment for high-risk teams.

---

## Why These Three (And Not Others)

| Chart | Audience | Decision It Unlocks |
|---|---|---|
| Spend vs. Limit (progress bars) | CFO, Finance | Immediate budget action per tool |
| Spend Trend (line + ceiling) | CFO, Board | Forecast & budget planning |
| Users by Dept (stacked bar) | IT Security, CISO | Risk prioritization & policy enforcement |

Together they cover the three governance axes:
- **Cost** (are we spending correctly?)
- **Trend** (where is spend going?)
- **Risk** (who is the security/compliance exposure?)

A pie chart of "spend by tool" is tempting but it answers a less urgent question (composition vs. risk).
A table of users is more exhaustive but harder to scan. These three charts minimize cognitive load
while maximizing decision velocity for the stakeholders who matter most.

