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
- instructors/     → highly detailed lesson notes, step by step
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

Primary corpus (2 Acts):
- Privacy Act 2020:           /act/public/2020/31/en/latest.xml
- Runner up: Impounding Act 1955: /act/public/1955/108/en/latest.xml

### Data source 2: NZ archival images (visual feature extraction notebook)
Source: Archives New Zealand collection via Wikimedia Commons
Category: "Category:Images from Archives New Zealand" (~9,000 images)
Licence: No known copyright restrictions (NZ Crown copyright, NZGOAL framework)
Requires: User-Agent header identifying the workshop


### Key data, reference, and inspiration for thematic analysis notebook
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
