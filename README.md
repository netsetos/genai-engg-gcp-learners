# GenAI on GCP — Learner Code

Companion code repo for the **Netsetos GenAI on GCP Capstone** course at [netsetos.com](https://netsetos.com).

> 12 modules · 42 lessons · production-grade Vertex AI / Gemini curriculum

---

## What's in this repo

| Folder | What it contains |
|---|---|
| `module-NN-slug/lesson-N.M-slug/notebooks/` | Runnable Colab exercises for the lesson |
| `module-NN-slug/lesson-N.M-slug/practice/` | Hands-on coding practice (rolling out over time) |
| `module-NN-slug/lesson-N.M-slug/interview/` | Coding interview prep (rolling out over time) |
| `requirements.txt` | Python dependencies (base) |
| `.env.example` | Environment variables you'll need |

> **Lesson theory, walkthroughs, practice-lab solutions, and interview Q&A live on [netsetos.com](https://netsetos.com).**
> This repo is intentionally **code-only** — open a notebook, run it, modify it, learn by doing.

---

## Getting started

### Option 1 — Colab (recommended)

1. Browse to any lesson's `notebooks/` folder
2. Click the **Open in Colab** badge at the top of the `.ipynb`
3. Set your GCP project: `Runtime → Run all` will prompt for `auth.authenticate_user()` on the first cell that needs it
4. Edit the `PROJECT_ID = 'documind-ai-YOUR-ID'` line in the notebook to your GCP project ID
5. Run all cells

See [SETUP.md](SETUP.md) for the full GCP project setup (one-time, ~20 min).

### Option 2 — Local Jupyter

```bash
git clone https://github.com/netsetos/genai-engg-gcp-learners.git
cd genai-engg-gcp-learners

python -m venv .venv
.venv\Scripts\Activate.ps1          # Windows PowerShell
# source .venv/bin/activate         # macOS / Linux

pip install -r requirements.txt

cp .env.example .env                # then set PROJECT_ID, REGION
gcloud auth application-default login
jupyter lab
```

See [SETUP.md](SETUP.md) for full details.

---

## Course modules

| # | Module | Lessons |
|---|---|---|
| 01 | Setup & IAM | Setup · IAM Security · First Gemini Call |
| 02 | Embeddings & Vector Stores | Token Economics · Embeddings · Firestore Vector · AlloyDB & BigQuery |
| 03 | Prompting & Routing | System Prompts · Structured Output · Model Routing |
| 04 | RAG (DIY → Managed) | Document AI · DIY RAG · RAG Engine · Search Grounding |
| 05 | BigQuery ML | BQML · Time Series · LLM in SQL · BigQuery → Vertex AI |
| 06 | Gemini Function Calling | Function Calling · Calling Loop · Parallel Tools |
| 07 | MCP & Cloud Run | FastMCP · Cloud Run Deploy · Agent + MCP |
| 08 | Agents (ADK + A2A) | Root Agent · Multi-Agent · Agent Engine · A2A |
| 09 | Multimodal & Pre-trained APIs | Multimodal · Generative Media · Pre-trained APIs |
| 10 | Tuning, Caching & Evaluation | SFT LoRA · Context Caching · Batch Routing · Evaluation |
| 11 | Self-hosting | Gemma on Cloud Run · Custom FastAPI · Hybrid LiteLLM |
| 12 | Production Deploy (DocuMind) | Infra Setup · RAG Backend · Admin Observability · Streamlit Frontend |

---

## What you'll need

- A **GCP project** with billing enabled — $300 free trial covers most of the course (see Lesson 1.1)
- **Python 3.11+** (only for local Jupyter; Colab handles this)
- **`gcloud` CLI** — pre-installed on Cloud Shell, or [install locally](https://cloud.google.com/sdk/docs/install)

You will **not** need API keys from any other provider — the course is GCP-native (Vertex AI / Gemini handle everything). Module 11 lesson 11.3 optionally uses LiteLLM with non-GCP fallbacks; the keys for those are explicit and skippable.

---

## Working through the course

Each lesson notebook is self-contained — install cell at top, then ~15-20 cells building up the lesson concepts. Expected runtime per notebook: 10-30 min.

Cost estimate: completing all 42 notebooks costs ~$5-15 in Gemini API spend if you stay on `gemini-2.5-flash` and don't repeat tuning runs. Module 10 (tuning) and Module 11 (self-hosting Gemma) have higher GPU costs — read the lesson 10.1 / 11.1 cost callouts before running.

---

## Structure

```
module-01-setup-and-iam/
└── lesson-1.1-setup/
    ├── notebooks/GCP_Capstone_1.1_Setup.ipynb
    ├── practice/README.md
    └── interview/README.md
```

Filenames preserve the original `GCP_Capstone_X.Y_<topic>.ipynb` convention used in the curriculum repo.

---

© 2026 Netsetos · Hyderabad, India · [netsetos.com](https://netsetos.com)
