# nlp_spam_project

Small NLP project to explore SMS spam detection using a classic `spam.csv` dataset.

## What this repo contains

- `data/spam.csv` — raw dataset used for the analysis.
- `notebooks/01_data_overview.ipynb` — Jupyter notebook that loads, cleans, and pre-processes the data.

## Prerequisites

- Ubuntu (local dev machine)
- Python 3.10+ and `venv` support

## Quick setup

1. Create and activate a virtual environment from the project root:

```bash
python3 -m venv venv
source venv/bin/activate
```

2. Install Python dependencies:

```bash
pip install -r requirements.txt
```

3. NLTK data (stopwords, punkt)

The notebook uses NLTK corpora. On some systems the NLTK downloader may try IPv6 first and stall — this repo includes instructions to install the corpora into the project's virtualenv so the notebook never needs to prompt or download at runtime.

Recommended (fast, reliable): download the corpora into the venv and point NLTK to that folder.

```bash
# run from project root (uses IPv4 to avoid IPv6 timeouts)
mkdir -p venv/nltk_data
curl -4 -L -o venv/nltk_data/stopwords.zip \
	https://raw.githubusercontent.com/nltk/nltk_data/gh-pages/packages/corpora/stopwords.zip
curl -4 -L -o venv/nltk_data/punkt.zip \
	https://raw.githubusercontent.com/nltk/nltk_data/gh-pages/packages/tokenizers/punkt.zip
unzip -o venv/nltk_data/stopwords.zip -d venv/nltk_data
unzip -o venv/nltk_data/punkt.zip -d venv/nltk_data
rm venv/nltk_data/*.zip

# Move unpacked folders into the locations NLTK expects (if needed)
mkdir -p venv/nltk_data/corpora venv/nltk_data/tokenizers
mv -n venv/nltk_data/stopwords venv/nltk_data/corpora/stopwords || true
mv -n venv/nltk_data/punkt venv/nltk_data/tokenizers/punkt || true
chmod -R a+rX venv/nltk_data
```

After that, the notebook will use the local copy. The project already sets the `NLTK_DATA` env var in the notebook so it points to `venv/nltk_data`:

See the notebook: [notebooks/01_data_overview.ipynb](notebooks/01_data_overview.ipynb)

## Verifying NLTK data is available

Run this to check that NLTK finds the two resources:

```bash
python3 - <<'PY'
import nltk
for p in ('corpora/stopwords','tokenizers/punkt'):
		try:
				print(p, 'FOUND at', nltk.data.find(p))
		except Exception as e:
				print(p, 'MISSING ->', e)
PY
```

## Running the notebook

1. Activate the virtualenv.
2. Start Jupyter Lab/Notebook:

```bash
pip install jupyterlab
jupyter lab
```

Open [notebooks/01_data_overview.ipynb](notebooks/01_data_overview.ipynb) and run the cells.

## Notes / Troubleshooting

- If downloads are slow or hanging, it's usually a network/IPv6 fallback issue — use the `curl -4` approach above.
- If you prefer the NLTK downloader, you can run:

```bash
python3 -m nltk.downloader -d venv/nltk_data stopwords punkt
```

- If you get permissions errors, ensure the virtualenv is owned by your user or use `chmod` as shown above.

If you want, I can commit further refinements (automated script to fetch data, or add a test script to verify environment).
