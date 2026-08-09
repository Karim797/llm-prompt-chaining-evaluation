# LLM Multi-Step Reasoning & Prompt Chaining Evaluation

An experiment that compares a **three-stage prompt chain** against a **single-shot baseline** on multi-step quantitative reasoning problems.

The prompt chain follows:

1. **Decompose** the problem into sub-tasks
2. **Solve** the sub-tasks step by step
3. **Synthesize** the final structured answer

## Project Goal

The project evaluates whether prompt chaining improves multi-step quantitative reasoning compared with asking the same model to solve the problem in one prompt.

Both approaches are evaluated using the same output contract to make the comparison fair.

## Evaluation

The notebook evaluates model outputs with:

- Exact-match numeric / field accuracy
- Fully-correct solve rate
- Parse success rate
- ROUGE-L
- Number of LLM calls
- Execution time

It also keeps intermediate chain outputs so failures can be inspected and error propagation can be analyzed.

## Supported Backends

The notebook provides one unified wrapper for:

- Hugging Face models
- Google Gemini
- Azure OpenAI

The default local model in the notebook is:

`google/flan-t5-base`

API credentials are read from environment variables rather than being hard-coded.

## Why Exact Match and ROUGE?

ROUGE measures text overlap, but text overlap alone is not enough for quantitative reasoning. A response can look linguistically similar while containing the wrong number.

For that reason, this project explicitly compares ROUGE with structured exact-match grading.

## How to Run

1. Clone the repository.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Open `multi_step_reasoning_prompt_chain.ipynb`.
4. Choose a backend in the configuration cell.
5. If using Gemini or Azure OpenAI, configure the required environment variables.
6. Run the experiment.

## Environment Variables

Depending on the backend, the notebook can use:

```text
GOOGLE_API_KEY
GEMINI_MODEL

AZURE_API_KEY
AZURE_ENDPOINT
AZURE_API_VERSION
AZURE_DEPLOYMENT
```

Never commit real API keys to GitHub.

## Repository Structure

```text
llm-prompt-chaining-evaluation/
├── multi_step_reasoning_prompt_chain.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Important Limitation

The notebook currently uses a very small evaluation set (`n = 4`) from one problem family. It is suitable as a portfolio experiment and demonstration of evaluation methodology, but it should not be presented as statistically conclusive.

The notebook itself recommends running at least two substantially different model backends before making claims about whether chaining improves reasoning.
