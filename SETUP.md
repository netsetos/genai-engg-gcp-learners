# Setup

Two ways to run the notebooks: **Colab** (zero install, fastest path) or **local Jupyter** (full control). Both authenticate to GCP via Application Default Credentials — no API keys to manage.

---

## Step 0 — One-time GCP project setup

This is the same setup walked through in **Lesson 1.1** (Setting Up Your GCP AI Project). Do this once before starting the course.

```bash
# 1. Create a project
gcloud projects create documind-ai-YOUR-ID --name="DocuMind AI Capstone"
gcloud config set project documind-ai-YOUR-ID

# 2. Link billing (required even for free-tier services)
gcloud billing projects link documind-ai-YOUR-ID \
  --billing-account=$(gcloud billing accounts list --format="value(name)" --limit=1)

# 3. Enable the APIs the course uses
gcloud services enable \
  aiplatform.googleapis.com run.googleapis.com firestore.googleapis.com \
  cloudbuild.googleapis.com secretmanager.googleapis.com storage.googleapis.com \
  documentai.googleapis.com speech.googleapis.com vision.googleapis.com \
  language.googleapis.com translate.googleapis.com bigquery.googleapis.com \
  artifactregistry.googleapis.com monitoring.googleapis.com logging.googleapis.com

# 4. (Strongly recommended) Set a budget alert at $25
gcloud billing budgets create \
  --billing-account=$(gcloud billing accounts list --format="value(name)" --limit=1) \
  --display-name="Capstone Safety Net" --budget-amount=25 \
  --filter-projects="projects/documind-ai-YOUR-ID" \
  --threshold-rule=percent=0.5 \
  --threshold-rule=percent=0.8 \
  --threshold-rule=percent=1.0
```

> **GCP free trial gives $300 + 90 days.** Resources STOP at trial end — your card is never auto-charged. The $25 budget above is a safety net, not a hard cap.

---

## Option 1 — Google Colab (recommended)

1. Browse to any notebook on GitHub (e.g. `module-01-setup-and-iam/lesson-1.1-setup/notebooks/GCP_Capstone_1.1_Setup.ipynb`)
2. Click the **Open in Colab** badge at the top
3. In the notebook, edit `PROJECT_ID = 'documind-ai-YOUR-ID'` to your project ID
4. Run cells. The first auth cell will pop up:
   ```python
   from google.colab import auth
   auth.authenticate_user()
   ```
   Approve the OAuth prompt with your GCP-billed Google account.
5. Run all subsequent cells

**No API keys needed for the core course.** Vertex AI uses your Colab Google account credentials via Application Default Credentials.

> Free Colab tier handles 90% of the course. Lessons 10.1 (LoRA tuning) and 11.1-11.3 (vLLM on Gemma) are GPU-heavy — Colab Pro's L4/T4 helps, or run them on Cloud Run GPU per the lesson instructions.

---

## Option 2 — Local Jupyter

### Prerequisites
- Python **3.11+** (3.10 will work, 3.11 recommended)
- `gcloud` CLI ([install](https://cloud.google.com/sdk/docs/install))
- ~2 GB free disk for dependencies

### Steps

```bash
# Clone
git clone https://github.com/netsetos/genai-engg-gcp-learners.git
cd genai-engg-gcp-learners

# Virtual environment
python -m venv .venv
.venv\Scripts\Activate.ps1          # Windows PowerShell
# .venv\Scripts\activate.bat        # Windows Cmd
# source .venv/bin/activate         # macOS / Linux

# Install base dependencies
pip install -r requirements.txt

# Authenticate gcloud + ADC
gcloud auth login
gcloud auth application-default login
gcloud config set project documind-ai-YOUR-ID

# Optional: set env vars for notebooks that read them
cp .env.example .env
# Edit .env in your editor — set PROJECT_ID, REGION

# Launch Jupyter
jupyter lab
```

Open any notebook and run it. Each notebook's first install cell adds lesson-specific packages on top of the base requirements.

---

## Authentication summary

| Where you are | Auth mechanism |
|---|---|
| **Colab** | `from google.colab import auth; auth.authenticate_user()` (cell in the notebook) |
| **Cloud Shell** | Pre-authenticated as your GCP user — nothing to do |
| **Cloud Workbench** | Pre-authenticated as the workbench's service account |
| **Local Jupyter** | `gcloud auth application-default login` (runs once, credential cached) |

You should **never need a service-account JSON key** for the course. If a lesson asks for one, it's only as an example of how to deploy — not for your dev environment.

---

## Common issues

**`Permission denied on resource project documind-ai-YOUR-ID`**
You forgot to set the project after creating it: `gcloud config set project documind-ai-YOUR-ID`.

**`API has not been used in project documind-ai-YOUR-ID before`**
The API isn't enabled. Re-run the `gcloud services enable ...` command from Step 0.

**`Quota exceeded` or `Billing account not found`**
Billing isn't linked to the project. Run the Step 0 billing-link command.

**Colab notebook can't find `google` module**
First cell of each lesson installs the right packages — make sure you ran the `!pip install -q ...` cell, not just the import cell.

---

## Lesson-specific extras

Some lessons need additional one-time setup beyond Step 0:

| Lesson | Extra setup |
|---|---|
| 2.4 (AlloyDB) | AlloyDB requires VPC + Private Services Access — the lesson walks through it; alternatively skip and use BigQuery vector search |
| 4.1 (Document AI) | Create a Document AI processor in the console — lesson 4.1 has the gcloud command |
| 4.4 (Search Grounding) | Enable Discovery Engine API + create a Search app |
| 7.2, 11.x, 12.x (Cloud Run) | Need Artifact Registry + a Docker daemon (or use Cloud Build) |
| 8.3 (Agent Engine) | Needs `aiplatform[agent_engines,adk]>=1.112` (lesson installs it) |
| 10.1 (LoRA tuning) | Tuning quota — request via console; runs on Vertex AI Tuning service |

---

## Cost expectations

| What you'll spend | Approx (USD) |
|---|---|
| Modules 1-6 (text-only Gemini) | $1-3 (covered by free tier easily) |
| Module 4 (Document AI + RAG Engine) | $1-3 (RAG Engine has a base cost) |
| Module 9 (multimodal + media gen) | $1-5 |
| Module 10 (tuning + evaluation) | $5-15 (LoRA tuning is the biggest single cost) |
| Module 11 (self-hosting Gemma on L4) | $2-10 if you keep services up briefly |
| Module 12 (full DocuMind deploy) | $1-3 if you tear down promptly |
| **Total** | **$15-40** — well under the $300 free-trial credit |

---

## Need help?

- Lesson Q&A: head to [netsetos.com](https://netsetos.com) for the published interview Q&A
- Issues with this repo's code: [GitHub Issues](https://github.com/netsetos/genai-engg-gcp-learners/issues)
- Course questions: sart@netsetos.com
