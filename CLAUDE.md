# CLAUDE.md — Embedding LLMs Workshop

## Project identity
This is a workshop repository for a workshop titled
"Programmatically using LLMs for qualitative research methods". It is developed by Dr Kyle Hemming (Centre for eResearch, University of Auckland) for researchers with no prior coding or ML background.
Target audience: researchers who use qualitative methods and want to understand how 
LLMs can be embedded into their workflows.
The priority is for researchers to learn by doing. They need relevant topics they can
relate the underlying concepts to.
Workshop length: 3 hours.

## Learning outcomes
- Build an LLM-assisted research workflow to aid analyses of qualitative data.
- Practice effective prompting techniques to prepare, label, and analyse unstructured text and images.
- Identify and mitigate key ethical risks, such as biases and hallucinations.
- Critically validate the outputs of LLM-assisted analysis methods.

## Repository structure
- notebooks/       → all workshop content. One .ipynb file per notebook.
- instructors/     → facilitation tips and timing notes
- learners/        → participant setup instructions
- profiles/        → learner personas

### Colab badge
Every notebook must include a Colab badge link in the first markdown cell, using this pattern:
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/UoA-eResearch/embedding-llms-qualitative-data-workshop/blob/main/notebooks/[filename].ipynb)
Replace [filename] with the actual notebook filename.

### Execution environment
All code is Python, implemented in Google Colab Jupyter notebooks.
Participants authenticate with their university Google account to access Colab —
no local installation required.

### LLM API: Groq free tier
- API key created on workshop day via groq.com using university Google account
  (OAuth — no email verification required)
- Primary model: "llama-3.3-70b-versatile" (text generation, structured outputs,
  thematic analysis)
- Vision model: "meta-llama/llama-4-maverick-17b-128e-instruct" (image analysis)
- Rate limits: generous free tier, unlikely to be hit in workshop exercises
- Single model variable defined at top of each notebook — swap in one place if
  model is deprecated

### Standard notebook setup cell (every notebook)
Every notebook begins with this cell:
    !pip install groq requests lxml Pillow
    import os, json, base64, requests, io
    from groq import Groq
    from lxml import etree
    from PIL import Image
    from IPython.display import Image as IPImage, display

    os.environ["GROQ_API_KEY"] = "paste_your_key_here"
    client = Groq(api_key=os.environ["GROQ_API_KEY"])
    TEXT_MODEL = "llama-3.3-70b-versatile"
    VISION_MODEL = "meta-llama/llama-4-maverick-17b-128e-instruct"

### Data source 1: NZ Legislation XML
No API key required. Direct XML access via URL pattern.
URL pattern: https://legislation.govt.nz/{type}/{category}/{year}/{number}/en/latest.xml
Key XML elements:
- <prov>  → individual sections
- <label> → section numbers
- <text>  → section content

Primary corpus (3 Acts):
- NZ Bill of Rights Act 1990: /act/public/1990/109/en/latest.xml
- Privacy Act 2020:           /act/public/2020/31/en/latest.xml
- Human Rights Act 1993:      /act/public/1993/82/en/latest.xml

Chosen because:
- Participants across disciplines recognise them
- Overlap thematically (rights, privacy, discrimination) — good for
  cross-corpus comparison exercises
- Vary in length and structure — useful for demonstrating scale challenges
- Legally and ethically clean for workshop use

Temporal comparison pairs (for longitudinal analysis exercises):
- Privacy Act 1993 (repealed) vs Privacy Act 2020
- Mental Health Act 1969 vs Mental Health Act 1992

### Data source 2: NZ archival images (visual feature extraction notebook)
Source: Archives New Zealand collection via Wikimedia Commons
Category: "Category:Images from Archives New Zealand" (~9,000 images)
Licence: No known copyright restrictions (NZ Crown copyright, NZGOAL framework)
Requires: User-Agent header identifying the workshop

### Research questions this infrastructure supports
RQ1 — What latent themes emerge from LLM-assisted
       coding of the NZ Privacy ACT? How do they compare using different persona prompts?
RQ2 — Visual feature extraction: what political claims and symbols appear in
       NZ archival protest images, and how does LLM analysis compare to
       archival metadata?

### Key data, reference
Use legislative data from: https://legislation.govt.nz/act/public/2020/31/en/latest.xml
Computational and Graph-Theoretic Analysis of Legislative Networks:
New Zealand's Mental Health Act as a Case Study.
Information (MDPI), February 2026.
https://www.mdpi.com/2078-2489/17/2/161
Demonstrates LLM-assisted topic modelling on NZ legislation specifically.
For images, use url = "https://commons.wikimedia.org/w/api.php" and resize to fit with Groq API limits (width = 400) and use model "meta-llama/llama-4-scout-17b-16e-instruct"

## Notebook Map (locked structure v1)

### Format and delivery
- 00 and 06: Powerpoint slides with instructor notes in instructors/
- 01-05: Google Colab Jupyter notebooks, one per notebook
- Notebooks live in notebooks/ folder in this repo
- GitHub Pages site links to each notebook directly
- Participants run notebooks in Colab — no local installation

### Designated cut on the day
Notebook 05 is the cut-off notebook if running behind.
Everything in 05 is covered conceptually in 03 and 04.
Participants lose a fun exercise, not a core concept.

### Notebook map

00 — Workshop setup (15 mins)
     Format: Powerpoint + instructor notes in instructors/00-setup.md
     Content: learning outcomes, research context, references, caveats
     Key message: "you can, not you must"
     Caveats to state explicitly:
     - I am not a qualitative researcher
     - Real methods, real data, real challenges
     - NOT advocating you must use this — showing what is possible
     - Ethics applications and supervisor conversations still required
     Outcomes: expectations set, instructor positionality established

01 — Environment setup (20 mins)
     Notebook: notebooks/01-environment-setup.ipynb
     Core concept: Colab, Python basics, Groq API connection
     Exercises:
     - Python cells as calculator
     - Object creation and print()
     - Groq key setup via Google OAuth
     - Run setup cell → see "connection successful"
     Outcomes: comfortable running cells, API connected, no fear of errors

02 — LLMs as research instruments (20 mins)
     Notebook: notebooks/02-llms-as-instruments.ipynb
     Core concept: what an LLM is, how to talk to one programmatically
     Exercises:
     - Converse with Groq in Python (free play, no dataset yet)
     - Observe temperature effects: run same prompt at temp=0 and temp=1
     - Introduce prompt structure: role / instruction / context / output format
     Outcomes: basic prompting syntax, understand temperature and
               consistency, mental model of LLM as instrument not oracle

---- BREAK (10 mins) ----

03 — Exploratory analysis with an LLM (30 mins)
     Notebook: notebooks/03-exploratory-analysis.ipynb
     Core concept: using an LLM to interrogate a dataset systematically
                   like a research assistant with implicit domain knowledge
     Dataset: Privacy Act 2020 fetched live as XML
              https://legislation.govt.nz/act/public/2020/31/en/latest.xml
     
     Exercise a [mandatory, ~10 mins]
     Obligation classifier: classify each section as "must" / "may" / "other"
     Validation moment: run same prompt twice, compare outputs
                        → introduce consistency as a validation concept
                        → discuss what temperature does to reproducibility
     
     Exercise b [if time, ~10 mins]
     Plain language summary: one sentence per section in plain English
     Validation moment: manually read 3 source sections, check LLM summaries
                        → introduce human spot-check as validation method
                        → discuss where LLM simplifies vs distorts
     
     Exercise c [stretch]
     Topic tagger: assign predefined tags to each section
                   (privacy, enforcement, definitions, exemptions, other)
     Stretch: design your own tags based on a research question you have
     
     Pedagogical design:
     - Fill-in-the-blanks prompt options in cell header comments
     - Paste output of final completed step into chat as completion signal
     - Slow progression: a → b → c, each builds on previous
     Outcomes: systematic prompting experience, first two validation
               techniques introduced (consistency check, human spot-check)

04 — Thematic analysis + validation (35 mins)
     Notebook: notebooks/04-thematic-analysis.ipynb
     Core concept: LLM-assisted thematic analysis grounded in published method
     Reference: Computational and Graph-Theoretic Analysis of Legislative
                Networks: NZ Mental Health Act as Case Study (MDPI, Feb 2026)
                https://www.mdpi.com/2078-2489/17/2/161
                Note: paper uses LDA + LLM summarisation — we do a tutorial-
                friendly prompting version practicing the same concepts
     Dataset: Human Rights Act 1993 fetched live as XML
              https://legislation.govt.nz/act/public/1993/82/en/latest.xml
     
     Exercise a [mandatory, ~10 mins]
     Inductive theme generation: ask LLM to generate themes from corpus
     Validation moment 1 — adversarial prompting: ask LLM to argue against
                           its own themes; discuss what changes and why
                           Reference: CHI 2025 qualitative LLM paper
     
     Exercise b [mandatory, ~10 mins]
     Persona experiment: run same analysis with two researcher personas
                         e.g. policy analyst vs Māori rights advocate
     Validation moment 2 — compare outputs side by side
                           Discussion question: "if outputs differ, which
                           is right? What does this mean for your methods
                           section?"
                           Key framing: differences are not right/wrong —
                           they reflect different research questions
     
     Exercise c [optional, ~10 mins]
     Choose your own persona from a provided list
     Run analysis, report one surprising difference back to the group
     
     Validation moment 3 — human spot-check at depth: manually read 3
                           source sections, assess whether themes are
                           supported by actual text
                           Reference: Tai et al. (2024) 10% spot-check
                           protocol
     Outcomes: inductive coding experience, understand prompt framing as
               methodological choice, know three named validation techniques:
               consistency check / adversarial prompting / human spot-check

---- BREAK (10 mins) ----

05 — Visual feature extraction (25 mins) [DESIGNATED CUT IF BEHIND]
     Notebook: notebooks/05-visual-extraction.ipynb
     Core concept: same prompting and validation principles from 03-04
                   applied to image data
     Dataset: Archives New Zealand via Wikimedia Commons API
              Category: Images from Archives New Zealand (~9,000 images)
              Licence: No known copyright restrictions, NZGOAL framework
              Curated corpus:
              - Prosecution_of_Strikers-_The_1913_Black_List_(29788240653).jpg
              - 1870_Petition_of_Unemployment_(9926603304).jpg
              - 1879_Petition_against_Steam_Trams_(29078906095).jpg
     
     Setup [pre-written, runs automatically, ~3 mins]
     All infrastructure cells pre-written — participant runs them,
     image loads and displays in notebook as payoff moment
     
     Exercise a [mandatory, ~8 mins]
     Structured JSON extraction from image
     Fill-in-the-blanks prompt options:
     - scene description
     - political claim
     - visible text
     - symbols present
     - confidence level
     
     Exercise b [choose one, ~10 mins]
     Apply one technique from 03-04 to image data:
     - Option 1: two research question prompts on same image, compare
                 (links to persona exercise in 04)
     - Option 2: adversarial prompt — challenge LLM's own description
                 (links to validation moment 1 in 04)
     - Option 3: compare LLM output against archival metadata
                 (links to human spot-check in 03 and 04)
     
     Outcomes: consolidate validation techniques across new data type,
               experience that image analysis uses identical principles
               to text analysis, "choose your own adventure" as autonomy

06 — Wrap up (10 mins)
     Format: Powerpoint + discussion
     Content:
     - Narrative of the day: EDA → thematic analysis → visual analysis
     - Validation techniques covered and their real-world references:
       consistency check / adversarial prompting / human spot-check
     - Where next: search your domain for LLM methods, ethics application,
                   UoA GPU cluster (Q2-Q3), paid APIs, Python fundamentals
     - Final message: "you have seen what is possible — the path forward
                      is yours to choose, at your own pace, on your terms"
     Outcomes: coherent narrative of the day, clear next steps,
               no pressure to continue

### Timing
00: 15 + 01: 20 + 02: 20 + break: 10 + 03: 30 + 04: 35
+ break: 10 + 05: 25 + 06: 10 = 175 mins
25 mins buffer for API issues, questions, slippage

## Writing style
- Content lives in notebook markdown cells, not standalone .md files.
- Plain English. Write for a researcher who has never seen a terminal.
- Active voice. Short sentences.
- Define every technical term on first use.
- No unexplained acronyms.
- When introducing a concept, use a concrete research workflow example
  (e.g. "imagine you have 500 interview transcripts...").
- Use standard markdown in notebook cells. Do not use Carpentries callout syntax
  (:::objectives, :::challenge, :::solution, :::keypoints blocks).

## Consistency rules
- "LLM" not "large language model" after first use"
- Workshop tone: collegial and low-stakes. Mistakes are expected.

## What I will ask you to do
- Create notebooks from descriptions
- Revise existing notebooks based on feedback notes I provide
- Ensure consistency in terminology and difficulty progression across notebooks
- Summarise external readings into workshop-appropriate explanations

## What you must NOT do
- Do not add R code blocks — all code is Python
- Do not invent citations or tool names
- Do not use Carpentries Workbench callout syntax in notebooks
