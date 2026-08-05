# FlyRank ML Internship — Phase 1 Setup Checklist

## Status: Local setup verified, browser steps remain

### What I've done already
- [x] Cloned starter repo to `C:\Users\sidda\workspace\flyrank-ml-internship-starter`
- [x] Installed all Python dependencies (pandas, numpy, scikit-learn, matplotlib, reportlab, duckdb, huggingface_hub)
- [x] Ran `scripts/run_all.py` successfully — pipeline completes in ~1 min
- [x] Verified outputs: baseline P@50 = 0.240, RF P@50 = 0.740 (3.1x lift)
- [x] Read SETUP.md, GUIDE.md, DATA_USE.md, and all assignment notebooks

### What you need to do (browser steps — I can't do these for you)

#### Step 1: Fork the repo to your own GitHub account (~2 min)
1. Go to https://github.com/flyrank-bih/flyrank-ml-internship-starter
2. Click **Use this template** button (top-right)
3. Select **Create a new repository**
4. Set to **public**
5. Name it anything (e.g., `flyrank-ml-internship`)
6. Click **Create repository**
7. Wait ~30 seconds, then refresh — an automatic commit rewires Colab badges

#### Step 2: Create Hugging Face account + request dataset access (~3 min)
1. Go to https://huggingface.co/join and create a free account
2. Go to https://huggingface.co/datasets/FlyRank/internship-warehouse
3. Fill the gate form — use **`FlyRank ML Internship 2026`** as affiliation
4. Tick the terms, click **Agree**
5. Go to huggingface.co → Settings → **Access Tokens** → Create new token → type **Read** → name it `internship` → copy it

#### Step 3: Run Notebook 01 in Colab (~15 min)
1. In **your** fork's README, click the **Open in Colab** badge for `01_first_look_and_discovery.ipynb`
2. Run all cells top to bottom
3. Do the **"Your turn"** cell — pick one discovery to write up
4. Save: **File → Save a copy in GitHub** (it should auto-select your repo)
5. Also: **File → Save a copy in Drive** (backup)

#### Step 4: Run Notebook 02 in Colab (~15 min)
1. Click the **Open in Colab** badge for `02_your_first_readable_model.ipynb`
2. Run all cells top to bottom
3. Do the **"Your turn"** cell — experiment with tree depth or features
4. Save to your repo again

#### Step 5: Verify your repo is ready
- Open your repo on github.com — both notebooks should be there with cell outputs visible
- If outputs are missing: back in Colab, **Runtime → Run all**, then **File → Save a copy in GitHub** again
- Your repo URL (format: `github.com/you/your-repo`) is your submission for Assignment 1

### Local setup (already done)
- Repo location: `C:\Users\sidda\workspace\flyrank-ml-internship-starter`
- All dependencies installed
- Pipeline runs locally: `python scripts/run_all.py`
- Outputs in `outputs/` directory
- Data in `data/raw/content_refresh_anonymized.csv` (30k rows, anonymized)

### Key files to know
| File | Purpose |
|---|---|
| `notebooks/01_first_look_and_discovery.ipynb` | Week 1 — run pipeline, make a discovery |
| `notebooks/02_your_first_readable_model.ipynb` | Week 2 — hand rule vs decision tree |
| `scripts/run_all.py` | Run the full pipeline locally |
| `work/notebooks/w01_research_question.ipynb` | Assignment 1 skeleton |
| `work/notebooks/w02_ml_task_framing.ipynb` | Assignment 2 skeleton |
| `work/README.md` | Rules for your work folder |
| `SETUP.md` | Full setup guide with pitfalls |
| `GUIDE.md` | Every file explained |
| `DATA_USE.md` | Data safety rules |

### Common pitfalls
- Don't create an empty GitHub repo by hand — use **Use this template** or push to an empty repo with no README (push will be rejected otherwise)
- Don't type your HF token in a code cell — your repo is public
- Don't commit datasets to git — CI will fail
- Don't use `colab.research.google.com` or `drive.google.com` links as submission — only `github.com/you/your-repo`
- If Colab badge opens the shared repo instead of yours, use **File → Open notebook → GitHub tab** and paste your repo URL