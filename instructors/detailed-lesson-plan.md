# Episode map (locked structure v2)

### Format and delivery
- 00 and 06: Powerpoint slides with instructor notes in episodes/
- 01-05: Google Colab Jupyter notebooks in notebooks/
- GitHub Pages landing page links directly to each notebook via Colab badge
- Participants run notebooks in Colab — no local installation

### Designated cut on the day
Episode 05 is the cut episode if running behind.
Participants lose a fun exercise, not a core concept.

### Narrative spine (paper reference throughout)
Every episode connects to Ardekani et al. (2026):
"Computational and Graph-Theoretic Analysis of Legislative Networks:
NZ Mental Health Act as Case Study" — MDPI Information, Feb 2026
https://www.mdpi.com/2078-2489/17/2/161

Episode → Paper component mapping:
01 → Ingestion (Step 1): XML parsing infrastructure
02 → Stochasticity finding: 86% vs 96% precision, temperature effects
03 → Extraction (Step 2): citation detection, rule-based vs LLM comparison
04 → Semantic Enrichment + Validation (Steps 4 + 5 + 6):
      theme generation, persona experiment, Jaccard validation
05 → Cross-modal application: same pipeline, image data

### Primary dataset
Privacy Act 2020 (used in NB01–NB04)
https://legislation.govt.nz/act/public/2020/31/en/latest.xml

### Fun Act for NB02 implicit knowledge exercise
Wool Board Disestablishment Act 2009
https://legislation.govt.nz/act/public/2009/62/en/latest.xml
Purpose: demonstrate that LLM implicit knowledge is patchy on obscure Acts
vs confident on well-known ones like the Privacy Act

### Episode map

00 — Workshop setup (15 mins)
     Format: Powerpoint + instructor notes in episodes/00-setup.md
     Key message: "you can, not you must"
     Paper intro: show the six-step workflow on a slide as the day's map
     Caveats: not a qualitative researcher; real methods real data;
              ethics and supervisor conversations still required
     Outcomes: expectations set, positionality established

01 — Environment setup (20 mins)
     Notebook: notebooks/01-environment-setup.ipynb
     Paper component: Step 1 — Ingestion infrastructure
     Core concept: Colab, Python basics, Groq API, XML parsing
     Exercises:
     - Python cells as calculator
     - Variables and print()
     - Groq API key setup via Google OAuth
     - Run setup cell → "Setup complete."
     Framing sentence: "Step 1 of the paper's workflow is ingestion —
     parsing the XML files NZ government publishes for every Act.
     By the end of this notebook you will have exactly that."
     Outcomes: environment working, API connected, no fear of errors

02 — LLMs as research instruments (20 mins)
     Notebook: notebooks/02-llms-as-instruments.ipynb
     Paper component: Stochasticity finding (86% vs 96% precision)
     Core concept: LLMs as probabilistic instruments, not oracles
     Exercises:
     a. Ask model about Privacy Act 2020 (no source data)
        — what does it know implicitly?
     b. Ask model about Wool Board Disestablishment Act 2009
        — contrast: knowledge is patchy on obscure Acts
     c. Temperature comparison: same prompt at temp=0 (twice)
        and temp=1 (twice)
        Discussion: "The paper measured LLM precision at 86% vs 96%
        for rule-based methods. You are now experiencing why."
     d. Introduce prompt structure: role / instruction / context /
        output format
     Framing sentence: "The paper found LLMs are less precise and
     less consistent than deterministic methods — temperature is
     exactly why. What you observe now is what the authors measured."
     Outcomes: understand temperature and stochasticity; mental model
               of LLM as instrument; basic prompt structure

---- BREAK (10 mins) ----

03 — Extraction and precision (30 mins)
     Notebook: notebooks/03-extraction-precision.ipynb
     Paper component: Step 2 — Extraction, Component 2 — Extraction
                      Dilemma (86% vs 96% precision)
     Dataset: Privacy Act 2020 XML (fetched live)
     
     Exercise a [mandatory, ~15 mins]:
     Citation detection — regex vs LLM
     - Regex: find all NZ Acts cited using pattern matching
       pattern = r'[A-Z][a-zA-Z\s]+Act\s+\d{4}'
     - LLM: "List all NZ Acts cited or referenced in this text"
     - Compare: true positives (both agree), LLM-only (hallucinations
       or implicit references), regex-only (LLM misses)
     - Manual check: verify 3-5 disputed cases against source XML
     Output: simple summary — "Regex found X. LLM found Y.
             They agreed on Z."
     
     Exercise b [discussion, ~10 mins]:
     Validation approaches — how would you do this at scale?
     - Idea: you and a colleague independently review X% of outputs
     - Idea: write your own summary and compare
     - If alignment: proceed with confidence
     - If not: back to drawing board — refine prompt or method
     Key references to cite:
     - Ardekani et al. (2026): 86% vs 96% precision finding
     - Tai et al. (2024): deductive coding with LLMs
     - EPJ Data Science scaling paper (Springer, 2025)
     
     Framing sentence: "You have done manually what the paper measured
     computationally. At 10 sections it is manageable. At 10,000 you
     need a protocol."
     Outcomes: understand extraction dilemma firsthand; know two
               named validation approaches; critical view of LLM
               precision before moving to deeper analysis

04 — Thematic analysis and validation (35 mins)
     Notebook: notebooks/04-thematic-analysis.ipynb
     Paper component: Steps 4+5+6 — Semantic Enrichment, Analysis,
                      Interpretation; Component 3 (LLM-assisted topic
                      modelling); Component 5 (Jaccard validation)
     Dataset: Privacy Act 2020 XML (same corpus, deeper analysis)
     Reference method: tutorial-friendly version of LLM-assisted LDA
                       from Ardekani et al. (2026) — same concepts,
                       simplified for 35-minute delivery
     
     Exercise a [mandatory, ~10 mins]:
     Inductive theme generation
     - Neutral prompt: "Identify 5-7 recurring themes in these sections.
       Focus on substantive themes and obligations. Exclude procedural
       boilerplate. Return JSON."
     - Output: theme list + 10-15 keywords per theme
     - This output feeds directly into exercises b and c
     
     Exercise b [mandatory, ~10 mins]:
     Persona experiment
     - Run same prompt with positioned persona:
       e.g. "You are a privacy rights advocate analysing
       surveillance risks in this legislation"
     - Compare keyword sets between neutral and positioned outputs
     - Discussion: "Which prompt is neutral? Is either neutral?
       What does your methods section need to say about this?"
     - Key framing: the tautology is deliberate and instructive —
       naming that a positioned prompt finds that persona's keywords
       is the methodological insight, not a flaw to hide
     
     Exercise c [mandatory, ~10 mins]:
     Jaccard validation
     - Pre-built COMMITTEE_KEYWORDS dictionary provided (see below)
     - Pre-written jaccard_similarity() function provided
     - Compare neutral theme keywords against each committee set
     - Compare positioned theme keywords against each committee set
     - Output: which committee does each theme align with?
     - Discussion: "Did your positioned prompt move the needle toward
       its target committee? What does that mean for validity?"
     - Framing: "The paper validated their clusters this way —
       comparing computationally derived communities against
       parliamentary committee domains. You just did the same."
     
     Optional exercise d [stretch]:
     Adversarial prompting — ask LLM to argue against its own themes
     Reference: CHI 2025 qualitative LLM paper
     
     Outcomes: inductive coding experience; understand prompt framing
               as methodological choice; know three validation
               techniques: consistency check / Jaccard / adversarial

---- BREAK (10 mins) ----

05 — Visual feature extraction (25 mins) [DESIGNATED CUT IF BEHIND]
     Notebook: notebooks/05-visual-extraction.ipynb
     Paper component: no direct equivalent — demonstrates pipeline
                      generalises beyond legislative text
     Dataset: Archives NZ via Wikimedia Commons API
              Curated corpus (confirmed working):
              - Prosecution_of_Strikers-_The_1913_Black_List_(29788240653).jpg
              - 1870_Petition_of_Unemployment_(9926603304).jpg
              - 1879_Petition_against_Steam_Trams_(29078906095).jpg
     
     Setup [pre-written, ~3 mins]:
     All infrastructure cells pre-written — participant runs them,
     image loads and displays as payoff moment
     
     Exercise a [mandatory, ~8 mins]:
     Structured JSON extraction from image
     Fill-in-the-blanks prompt options provided:
     - scene description
     - political claim
     - visible text
     - symbols
     - keywords (10-15) ← feeds into exercise b
     - confidence level
     
     Exercise b [mandatory, ~10 mins]:
     Cross-modal Jaccard validation
     - Pre-written jaccard_similarity() function (same as NB04)
     - Compare image keywords against NB04 theme keywords
     - Compare image keywords against COMMITTEE_KEYWORDS
     - Output: "This image most aligns with [X] committee and
       [Y] theme"
     - Discussion: "Does the model's assessment match what you see?
       What does a discrepancy tell you?"
     - Framing: "You used the same extraction and validation pipeline
       on an image that you used on text. The data type changed.
       The methodology did not."
     
     Optional exercises [take-home, linked from GitHub Pages]:
     - Try a different image from WORKSHOP_IMAGE_CORPUS
     - Run with a positioned prompt, compare Jaccard scores
     - Try adversarial prompting on an image description
     
     Outcomes: consolidate validation techniques across data types;
               see pipeline generalises beyond text

06 — Wrap up (10 mins)
     Format: Powerpoint + discussion
     Return to six-step workflow slide from episode 00
     Walk through: participants did every step today
     Validation recap: consistency check / Jaccard / adversarial /
                       human spot-check — all citable methods
     Where next: search your domain, ethics application,
                 UoA GPU cluster Q2-Q3, paid APIs, Python fundamentals
     Final message: "You have seen what is possible. The path forward
                    is yours to choose."

### Timing
00: 15 + 01: 20 + 02: 20 + break: 10 + 03: 30 + 04: 35
+ break: 10 + 05: 25 + 06: 10 = 175 mins
25 mins buffer for API issues, questions, slippage

### COMMITTEE_KEYWORDS (fixed asset — use in NB04 and NB05)
```python
COMMITTEE_KEYWORDS = {
    "Justice Committee": [
        "criminal", "justice", "court", "offence", "penalty",
        "prosecution", "enforcement", "police", "imprisonment",
        "tribunal", "legal", "rights", "appeal", "sentence", "conviction"
    ],
    "Health Committee": [
        "health", "medical", "treatment", "patient", "clinical",
        "disease", "mental", "disability", "care", "hospital",
        "practitioner", "pharmaceutical", "wellbeing", "public health",
        "safety"
    ],
    "Environment Committee": [
        "environment", "resource", "land", "water", "conservation",
        "climate", "emissions", "biodiversity", "sustainable",
        "pollution", "ecological", "planning", "consent", "impact",
        "natural"
    ],
    "Finance and Expenditure Committee": [
        "financial", "revenue", "tax", "expenditure", "budget",
        "fiscal", "economic", "payment", "fund", "investment",
        "commercial", "income", "cost", "penalty", "levy"
    ],
    "Governance and Administration Committee": [
        "privacy", "information", "data", "personal", "agency",
        "public", "government", "official", "disclosure", "access",
        "transparency", "accountability", "complaint", "commissioner",
        "record"
    ],
    "Social Services and Community Committee": [
        "social", "community", "welfare", "family", "housing",
        "poverty", "employment", "education", "youth", "elderly",
        "disability", "support", "benefit", "vulnerable", "protection"
    ]
}
```

### Known risk points
| Risk | Mitigation |
|------|-----------|
| Groq OAuth fails | Backup keys ready; walk through signup on projector |
| Rate limit hit | Unlikely free tier; if hit, participants share output |
| XML fetch fails | Pre-load Privacy Act sections as fallback variable |
| NB05 over time | Designated cut — skip cleanly |
| Participants stuck | Encourage skipping and continuing — errors expected |
| Jaccard function errors | Pre-test function in both NB04 and NB05 before delivery |
