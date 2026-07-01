---
title: "Before the Workshop: Setup"
---

This page tells you everything you need to do before the workshop.
There is no software to install. You need two things: a browser and your university Google account.

The whole process takes about 5 minutes.

---

## Step 1: Make sure you can open Google Colab

Google Colab is a free coding environment that runs in your browser.
You write and run code in a notebook — a document made up of cells you can run one at a time.

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Sign in with your **university Google account** (e.g. `yourname@auckland.ac.nz`)
3. You should see the Colab welcome screen

:::::::::::::::::::::::::::::::::::::::: callout

### No university Google account?

A personal Gmail account also works.
If you have neither, create a free Gmail account before the workshop.

::::::::::::::::::::::::::::::::::::::::::::::::

---

## Step 2: Create a free Groq API key

Groq provides free access to powerful language models.
You will use this key to send text to an LLM and receive responses.

1. Go to [console.groq.com](https://console.groq.com)
2. Click **Sign in with Google** and use the same Google account you used for Colab
3. You will be taken straight to your dashboard — no email verification required
4. In the left sidebar, click **API Keys**
5. Click **Create API Key**
6. Give it any name (e.g. `workshop`)
7. **Copy the key now** — you will not be able to see it again after you close this window

:::::::::::::::::::::::::::::::::::::::: callout

### Keep your key safe

Your API key is like a password. Do not share it or post it publicly.
For this workshop, you will paste it directly into a notebook cell.
That is fine — the notebook is private to your Google account.

::::::::::::::::::::::::::::::::::::::::::::::::

---

## Step 3: Run the test cell

This confirms that Colab and your Groq key are working before the workshop starts.

1. Open this notebook: **[FIXME: insert Colab link]**
2. Find the cell below and paste your Groq API key where it says `paste_your_key_here`
3. Click the cell, then press **Shift + Enter** to run it

```python
!pip install groq requests lxml Pillow -q

import os
from groq import Groq

os.environ["GROQ_API_KEY"] = "paste_your_key_here"
client = Groq(api_key=os.environ["GROQ_API_KEY"])
TEXT_MODEL = "llama-3.3-70b-versatile"

response = client.chat.completions.create(
    model=TEXT_MODEL,
    messages=[{"role": "user", "content": "Reply with exactly: Setup complete."}]
)
print(response.choices[0].message.content)
```

If everything is working, you will see:

```
Setup complete.
```

That is all you need to do before the workshop.

:::::::::::::::::::::::::::::::::::::::: callout

### Something went wrong?

Check these things in order:

1. Did you paste the full key? It starts with `gsk_` and is about 56 characters long.
2. Did you run the cell? Click inside it and press **Shift + Enter**.
3. Is the key inside the quote marks? It should look like `"gsk_abc123..."` not `"paste_your_key_here"`.

If none of these help, bring the error message to the workshop and we will fix it together in the first 5 minutes.

::::::::::::::::::::::::::::::::::::::::::::::::

---

## On the day: the first 10 minutes

At the start of the workshop, before any content begins, the instructor will say:

> **"Here's the only things you need to know today."**

They will then live-demonstrate four things in the notebook — not to teach you Python,
but to show you how a notebook behaves. This takes about 8 minutes.

**1. Running a cell**
Click a cell, press Shift+Enter. It runs. That's it.

**2. Changing a value**
Change a word or a number in the cell, run it again. The output updates.
The notebook does exactly what you tell it.

**3. Triggering an error — on purpose**
The instructor will break something deliberately and read the error message aloud.
The last line of the error message tells you what went wrong.
They will then fix it.

This is the most important minute of the whole workshop. Errors are not failures.
They are information. Every programmer sees them constantly. Reading the last line
and fixing it is the whole skill.

**4. Seeing inside the notebook**
The instructor will show a `print()` statement — a way to look at the value of
any variable at any point. If you are ever unsure what your code is doing, print it.

After these four things, you know enough to follow every exercise in the workshop.
