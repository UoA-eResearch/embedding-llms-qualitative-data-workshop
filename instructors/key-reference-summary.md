This paper introduces a computational framework for constructing and analysing **focal legislative networks**, using New Zealand’s *Mental Health (Compulsory Assessment and Treatment) Act 1992* as a seed case study. The methodology is designed to help researchers "zoom in" on a specific area of interest within a massive legal corpus, integrating **graph theory** with **LLM-assisted semantic modelling**.

For your workshop, you can frame the exercises around these five core components:

### 1. Seeded, Depth-Limited Network Construction
The paper argues that whole-corpus analyses often bury domain-specific Acts under globally dominant hubs (like constitutional or procedural laws). 
*   **The Concept:** Start with a "seed node" (a specific Act) and perform a **layer-by-layer expansion**. 
*   **The Logic:** The authors found that by the third layer of expansion, the focal Act (Mental Health) already lost its centrality, representing 34% of the entire legislative corpus. 
*   **Workshop Exercise:** Have participants select a "seed" and programmatically build 1-hop, 2-hop, and 3-hop networks to see how the "thematic relevance" dilutes as depth increases.

### 2. The Extraction Dilemma: Rule-Based vs. LLM-Based
A major theme is the trade-off between **deterministic rule-based extraction** and **probabilistic LLM-based extraction**.
*   **The Findings:** While LLMs can identify relationships without explicit rule-writing, they exhibit **stochasticity** (inconsistent results across runs) and lower precision (86%) compared to custom rule-based engines (96%).
*   **The Goal:** High-precision extraction of six relation types: Amendment, Repeal (full/partial), and Citation.
*   **Workshop Exercise:** Provide a snippet of XML legislative text. Task 1: Use regex to extract citations. Task 2: Use an LLM to do the same. Task 3: Compare precision and discuss the "cost of inconsistency" in qualitative research.

### 3. LLM-Assisted Topic Modelling (Semantic Node Embedding)
The paper proposes a novel "LLM-assisted LDA" approach to move beyond simple citation counts.
*   **The Method:** Instead of running topic models (LDA) on raw, "noisy" legislative text full of procedural boilerplate (e.g., "section," "subsection"), the authors use an LLM to generate **compact executive summaries and keywords** for each Act.
*   **The Advantage:** This "semantic abstraction" produces topics with much higher coherence and cleaner separation.
*   **Workshop Exercise:** Prompt an LLM to summarise a complex statute for "substantive themes" while explicitly excluding "procedural language". Then, have participants compare the resulting topic clusters to those generated from raw text.

### 4. Structural Metrics for Qualitative Impact
The paper applies specific graph-theoretic measures to interpret legal influence:
*   **Katz Prestige Centrality:** Models influence by counting all directed paths to a node, down-weighting longer paths. This is used to identify "procedural cascade risk"—how changes in one Act necessitate changes in others.
*   **Louvain Community Detection:** Groups Acts into communities based on their density of interconnections.
*   **Workshop Exercise:** Use these metrics to identify "structural bottlenecks"—Acts that are highly central locally but perhaps not globally—and discuss what this means for policy reform.

### 5. Validation and Institutional Alignment
The authors validate their computational findings by comparing the detected "communities" of Acts to real-world **parliamentary committee domains** using **Jaccard similarity**.
*   **The Concept:** Measuring the lexical overlap between LDA-derived keywords and predefined committee keyword sets.
*   **The Application:** Identifying where policy domains overlap, such as the interface between mental health, policing, and criminal procedure.
*   **Workshop Exercise:** Map a detected cluster of Acts back to a qualitative "policy domain" and use Jaccard similarity to see if the "code" matches "reality".

### Summary of Workflow for Workshop Prompting:
To prompt your model for exercises, you can use this logical flow from the paper:
1.  **Ingestion:** Parse XML to extract metadata and text.
2.  **Extraction:** Identify the "edges" (citations/amendments).
3.  **Expansion:** Build the "focal" subgraph from a seed.
4.  **Semantic Enrichment:** Use an LLM to clean and summarise nodes for topic modelling.
5.  **Analysis:** Calculate Katz centrality and detect Louvain communities.
6.  **Interpretation:** Align clusters with external institutional frameworks (committees).