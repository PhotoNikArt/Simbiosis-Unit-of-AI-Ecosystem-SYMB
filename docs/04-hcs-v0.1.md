# HCS v0.1 — Human Capacity Score

> *Working draft. Invitation to critique of formulas, not of meaning.*
> *Simbiosis Unit of AI Ecosystem*

---

## What It Measures

How capable a human remains of acting and deciding **independently** in the presence of AI. Not "does the system work," but "does the human remain in shape."

---

## Formula

**HCS = (CA + DS + MC) / 3**

Three components, each normalized to [0, 1].

---

## Component 1: CA — Cognitive Autonomy

**What:** The share of decisions a human makes without mandatory AI prompting.

**Data source:**
- Surveys (representative sample per sector, 2 times a year)
- System logs where possible (e.g., share of diagnoses made by a doctor without AI recommendation)

**How to calculate:**
CA = (number of decisions without AI) / (total number of decisions)

**Threshold from SU-AI-01:** ≥ 0.40

**Limitation:** Self-assessment in surveys is unreliable. Logs are more accurate, but not available everywhere.

---

## Component 2: DS — Demographic Stability

**What:** Whether the population is falling faster than acceptable.

**Data source:**
- National statistics (Rosstat, Eurostat, UN Data)
- Public APIs: World Bank, UN Population Division

**How to calculate:**
DS = 1 − (actual_decline / 0.15)

Where:
- actual_decline = (population_now − population_15_years_ago) / population_15_years_ago, if negative
- 0.15 = maximum allowable decline (15% over 15 years, from SU-AI-01)

**Examples:**
- Decline of 7.5% → DS = 0.50
- Decline of 15% → DS = 0
- Growth or stability → DS = 1.00

**Limitation:** Demography is inertial. 15 years is a long horizon. A faster indicator (year-on-year birth rate) is needed for quick reactions.

---

## Component 3: MC — Manual Control

**What:** The share of critical systems that can be controlled by a human without mandatory AI participation.

**Data source:**
- National critical infrastructure registries
- Operator certification (is there a manual control skill?)

**How to calculate:**
MC = (number of systems with full manual mode + 0.5 × number of systems with AI-assist) / total number of systems

Categories:
- Human-only = 1.0
- Human-with-AI-assist = 0.5
- AI-only = 0

**Threshold from SU-AI-01:** ≥ 0.30

**Limitation:** Critical infrastructure registries are often closed. In v0.1 — expert estimates.

---

## Regional Level: Adding WS

### The Problem with DS at the Regional Level

Demographic Stability (DS) measures population change. This works at the national level — internal migration does not change the total pool of specialists.

But at the regional level, DS breaks down. In sectors where specialists can migrate across regions or countries (energy, finance, IT, communications), regional demographics do not directly determine workforce availability. A region can lose population while its power grid runs smoothly — because operators are brought in from elsewhere.

DS alone is not enough for regional measurement. A second component is needed.

---

## Component 4: WS — Workforce Stability

**What:** The stability of the workforce actually operating the critical systems.

**Data source:**
- Operator turnover rates (annual)
- Average tenure in the sector
- Vacancy fill rate

**How to calculate:**

WS = 1 − (actual_turnover / maximum_turnover)

Where:
- actual_turnover = annual turnover rate of critical operators
- maximum_turnover = 0.15 (15% per year — initial estimate, subject to revision)

**Examples:**
- Turnover of 7.5% → WS = 0.50
- Turnover of 15% → WS = 0
- Turnover below 3% → WS ≈ 1.00

**Limitation:** Turnover data is often internal and not public. In v0.1 — expert estimates.

---

## Updated Formula

**National level:**
HCS = (CA + DS + MC) / 3

**Regional level:**
HCS = (CA + DS + WS + MC) / 4

**Why both DS and WS at regional level:**
- DS reflects long-term demand, tax base, and the total national pool of specialists.
- WS reflects immediate operational continuity — are the people who run the systems staying?

A region can have stable population but high turnover of operators (WS low). Or falling population but stable workforce (WS high). Both matter.

---

## Sectoral Applicability

| Sector | Third component | Fourth component (regional) |
|--------|-----------------|----------------------------|
| Medicine | DS (national) | WS (regional) |
| Energy | DS (national) | WS (regional) |
| Communications | DS (national) | WS (regional) |
| Finance | DS (national) | WS (regional) |
| Governance | DS + WS | DS + WS |

DS is always applied at the national level. WS is always applied at the regional or sectoral level.

---

## Recalculated Example — Energy (Regional)

**CA:** 0.635 (from the earlier example)

**DS (national):** 0.40

**WS (regional):**
- Operator turnover: 8% per year
- Maximum: 15%
- WS = 1 − (0.08 / 0.15) = 0.47

**MC:** 0.58

**Regional HCS = (0.635 + 0.40 + 0.47 + 0.58) / 4 = 2.085 / 4 = 0.521**

**Zone:** Warning (0.50–0.69).

**Interpretation:**
- CA and MC are in normal range.
- DS (0.40) shows long-term demographic pressure.
- WS (0.47) shows moderate turnover — operators change faster than ideal for experience transfer.
- Both DS and WS pull the index down. Addressing only one will not be enough.
- 
- ## Trigger Thresholds

| HCS | Zone | Action |
|---|---|---|
| ≥ 0.70 | Safe | AI deployment permitted |
| 0.50–0.69 | Warning | Mandatory audit, restriction of new deployments |
| 0.35–0.49 | Restriction | AI autonomy limited, priority on human restoration |
| < 0.35 | Prohibited | New deployment forbidden |

---

## Example Calculation (conventional sector — urban medicine)

**CA:** out of 1000 diagnostic decisions, 450 made by a doctor without AI prompting → **CA = 0.45**

**DS:** city population declined by 6% over 15 years → DS = 1 − (0.06 / 0.15) = **0.60**

**MC:** out of 20 critical systems, 8 — full manual mode, 6 — with AI-assist, 6 — AI only → MC = (8×1.0 + 6×0.5 + 6×0) / 20 = (8 + 3 + 0) / 20 = **0.55**

**HCS = (0.45 + 0.60 + 0.55) / 3 = 0.533**

**Result:** Warning zone. Audit mandatory. New AI deployments in the sector restricted until recovery.

---

## What is Not Resolved in v0.1 (honestly)

1. **Component weights.** Currently equal (1/3). Perhaps cognitive autonomy should weigh more — but this is a subject for discussion.
2. **Subjectivity of CA.** Surveys can be distorted. Logs are not available everywhere.
3. **Sectoral vs global HCS.** Count by sectors or by country?
4. **Verification.** Who checks the data? In v0.1 — public sources, but they can be challenged.
5. **Dynamics.** HCS is a snapshot. Is a trend needed, not just a point?

---

## Next Step

Take **one real sector** (e.g., medicine in your region) and calculate HCS manually. At least approximately.

If the formula gives a meaningful result on real data — it works. If not — we will fix it.

---

📜 **SIMBIOSIS UNIT OF AI ECOSYSTEM**
*Earth 2.0 — Hybrid Era*
