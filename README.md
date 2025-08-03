# 🧠 PaperCoder: Extract Code from Research Papers

This guide explains how to run PaperCoder using either OpenAI models (like `o3-mini`) or open-source alternatives via `vLLM`. It also covers setup, dataset details, and how to evaluate the quality of the generated repositories.

---

## 🔧 Environment Setup

Install dependencies based on your use case:

```bash
# For OpenAI API
pip install openai

# For vLLM-based local inference
pip install vllm

# Optional: Install everything from a single requirements file
pip install -r requirements.txt

📁 Folder Structure (Key Outputs)

          outputs/
├── Transformer/
│   ├── analyzing_artifacts/
│   ├── coding_artifacts/
│   └── planning_artifacts/
└── Transformer_repo/   # Final code repository generated

📄 Convert PDF to JSON (if not using LaTeX)
If your paper is in PDF format, convert it into structured JSON using s2orc-doc2json.

git clone https://github.com/allenai/s2orc-doc2json.git
cd s2orc-doc2json/grobid-0.7.3
./gradlew run

Then process your PDF:

python ../doc2json/grobid2json/process_pdf.py \
    -i <your_paper.pdf> \
    -t ../temp_dir \
    -o ../output_dir/paper_coder

🚀 Run PaperCoder
Option 1: Using OpenAI API (o3-mini)
💰 Approximate cost: $0.50–$0.70 per paper

   export OPENAI_API_KEY="your_openai_key"

# For JSON version of the paper
cd scripts
bash run.sh

# For LaTeX source
bash run_latex.sh

Option 2: Using Open-Source Model with vLLM
Default: deepseek-ai/DeepSeek-Coder-V2-Lite-Instruct

# For JSON paper format
cd scripts
bash run_llm.sh

# For LaTeX source
bash run_latex_llm.sh

📚 Paper2Code Dataset
PaperCoder uses the Paper2Code dataset for benchmarking.

Detailed metadata: data/paper2code/

See Section 4.1 of the official paper for more.

🧪 Evaluation: Generated Code Quality
🔨 Setup

  pip install tiktoken
export OPENAI_API_KEY="your_openai_key"

🧾 Reference-Free Evaluation

  cd codes/
python eval.py \
    --paper_name Transformer \
    --pdf_json_path ../examples/Transformer_cleaned.json \
    --data_dir ../data \
    --output_dir ../outputs/Transformer \
    --target_repo_dir ../outputs/Transformer_repo \
    --eval_result_dir ../results \
    --eval_type ref_free \
    --generated_n 8 \
    --papercoder

📌 Reference-Based Evaluation

 cd codes/
python eval.py \
    --paper_name Transformer \
    --pdf_json_path ../examples/Transformer_cleaned.json \
    --data_dir ../data \
    --output_dir ../outputs/Transformer \
    --target_repo_dir ../outputs/Transformer_repo \
    --gold_repo_dir ../examples/Transformer_gold_repo \
    --eval_result_dir ../results \
    --eval_type ref_based \
    --generated_n 8 \
    --papercoder

📊 Example Evaluation Output

 ========================================
📄 Paper: Transformer
🧪 Evaluation Type: ref_based
✅ Score: 4.5
📈 Valid Results: 8/8
💵 Total Cost: ~$0.16
========================================

