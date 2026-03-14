# MedExtract Reliability Suite - Full Setup Guide

This guide helps a new contributor set up the project from scratch, configure API keys, and run the pipeline.

## 1. Prerequisites

- macOS, Linux, or Windows
- Python 3.9+
- Git
- Internet access for API calls

Check your versions:

```bash
python3 --version
git --version
```

## 2. Clone the Repository

```bash
git clone https://github.com/ansh-codr/MedExtract-Reliability-Suite.git
cd MedExtract-Reliability-Suite
```

## 3. Create and Activate Virtual Environment

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Windows (PowerShell)

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

## 4. Install Dependencies

```bash
pip install -r requirements.txt
```

## 5. Configure Environment Variables

Copy the template:

```bash
cp .env.example .env
```

Edit `.env` and set the provider + key you want.

Supported values:

- `LLM_PROVIDER=gemini`
- `LLM_PROVIDER=openai`
- `LLM_PROVIDER=openrouter`
- `LLM_PROVIDER=mistral`

### Example `.env` for Mistral

```env
LLM_PROVIDER=mistral
MISTRAL_API_KEY=your_mistral_api_key_here
MISTRAL_MODEL=mistral-small-latest

FUZZY_MATCH_THRESHOLD=80
TEST_DATA_DIR=test_data
OUTPUT_DIR=output
```

### Example `.env` for OpenRouter

```env
LLM_PROVIDER=openrouter
OPENROUTER_API_KEY=your_openrouter_api_key_here
OPENROUTER_MODEL=openai/gpt-4o-mini

# Optional but recommended headers
OPENROUTER_SITE_URL=https://your-site.example
OPENROUTER_APP_NAME=MedExtract Reliability Suite

FUZZY_MATCH_THRESHOLD=80
TEST_DATA_DIR=test_data
OUTPUT_DIR=output
```

## 6. Where to Get API Keys

- Gemini API key: https://aistudio.google.com/
- OpenAI API key: https://platform.openai.com/api-keys
- OpenRouter API key: https://openrouter.ai/keys
- Mistral API key: https://console.mistral.ai/

Notes:

- OpenRouter may require account credits before requests succeed.
- Keep `.env` private and never commit secrets.

## 7. Data Setup

You can run with either mock data (quick test) or full dataset.

### Option A: Quick Local Mock Test

Generate mock files:

```bash
python create_mock_data.py
```

This creates:

- `test_data/patient_001/patient_001.md`
- `test_data/patient_001/patient_001.json`
- `output/patient_001.json` (mock prediction)

### Option B: Full Dataset

Download dataset from:

- https://drive.google.com/drive/folders/1Elnuj6n7QDazhmSsgCMkL1g9UOBTEdkQ

Place folders in `drive/` (or change `TEST_DATA_DIR` in `.env`).

## 8. Run Commands

## 8.1 Full pipeline (extract + evaluate + report)

```bash
python run_pipeline.py
```

## 8.2 Evaluation only (skip extraction)

```bash
python run_pipeline.py --skip-llm --test-data test_data --output-dir output
```

## 8.3 Submission test entrypoint

```bash
python test.py input.json output.json
```

## 9. Output Files

After a run:

- `output/evaluation_report.json` - machine-readable metrics
- `output/error_heatmap.png` - heatmap image
- `report.md` - human-readable report

## 10. Provider Switching

Change provider in `.env`:

```env
LLM_PROVIDER=gemini
```

or

```env
LLM_PROVIDER=openai
```

or

```env
LLM_PROVIDER=openrouter
```

or

```env
LLM_PROVIDER=mistral
```

Then run:

```bash
python run_pipeline.py --test-data test_data --output-dir output
```

## 11. Troubleshooting

### Error: `...API_KEY not set in .env`

- Ensure `.env` exists in repo root.
- Confirm the correct key variable for chosen provider is set.

### OpenRouter 402 insufficient credits

- Add credits at https://openrouter.ai/settings/credits
- Verify key belongs to funded account.

### Module import errors

Reinstall dependencies:

```bash
pip install -r requirements.txt
```

### Verify Python files compile

```bash
python -m py_compile src/llm_extractor.py src/evaluator.py src/heatmap.py run_pipeline.py test.py
```

## 12. Security Best Practices

- Do not commit `.env`.
- Rotate API keys if they are exposed.
- Use separate keys for development and production.

## 13. Quick Start (Copy/Paste)

```bash
git clone https://github.com/ansh-codr/MedExtract-Reliability-Suite.git
cd MedExtract-Reliability-Suite
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# edit .env with your provider and API key
python create_mock_data.py
python run_pipeline.py --skip-llm --test-data test_data --output-dir output
```
