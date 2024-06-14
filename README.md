# FinanceBench: A New Benchmark for Financial Question Answering

<p align="center">
    <img src="fig1.png" alt="drawing" style="width: 400px; display: block; margin: 0 auto; text-align:center;"/>
</p>

**Abstract:** 
FinanceBench is a first-of-its-kind test suite for evaluating the performance of LLMs on open book financial question answering (QA). This repository contains an open source sample of 150 annotated examples used in the evaluation and analysis of models assessed in the FinanceBench paper. FinanceBench comprises 10,231 questions about publicly traded companies, with corresponding answers and evidence strings. The questions in FinanceBench are ecologically valid and cover a diverse set of scenarios. They are intended to be clear-cut and straightforward to answer to serve as a minimum performance standard. We test 16 state of the art model configurations (including GPT-4-Turbo, Llama2 and Claude2, with vector stores and long context prompts) on a sample of 150 cases from FinanceBench, and manually review their answers (n=2,400). The cases are available open-source. We show that existing LLMs have clear limitations for financial QA. Notably, GPT-4-Turbo used with a retrieval system incorrectly answered or refused to answer 81\% of questions. While augmentation techniques such as using longer context window to feed in relevant evidence improve performance, they are unrealistic for enterprise settings due to increased latency and cannot support larger financial documents. We find that all models examined exhibit weaknesses, such as hallucinations, that limit their suitability for use by enterprises.

[![arXiv](https://img.shields.io/badge/arXiv-2311.11944-<COLOR>.svg)](https://arxiv.org/abs/2311.11944)

**Contact:**
To evaluate your models on the full `FinanceBench` dataset, or if you have questions about this work, you can email us at contact@patronus.ai

---

**Dataset Overview:**
The provided open-source dataset (n=150) consists of two JSONL files (located in `/data/`):


**`financebench_open_source.jsonl`**:
```text
    - financebench_id (int):            Unique identifier of the question
    - question (str):                   Question of interest
    - answer (str):                     Human-annotated gold answer
    - dataset_subset_label (str):       Label to identify in which data subset the question is present ("OPEN_SOURCE" or "CLOSED_SOURCE")
    - evidence (list[dict])             List of EvidenceDict's. 
    - justification (str)               Human-Annotated justification of the gold answer
    - question_type (str)               Type of Question: 'metrics-generated', 'domain-relevant', 'novel-generated' 
    - question_reasoning (str)          Reasoning Type needed to solve the question
    - domain_question_num (str)         ID of domain-relevant questions (`dg01` to `dg25`), "None" for 'metrics-generated' and 'novel-generated' questions
    - company (str)                     Company of Interest
    - doc_name (str)                    Unique Document Identifier. Format: {COMPANY}_{PERIOD}_{TYPE}. Some exceptions have the format {COMPANY}_{PERIOD}_{TYPE}_dated-{DATE}


    Each EvidenceDict contains four fields: 
        - "evidence_text" (str):            Extracted evidence text from annotators (sentence, paragraph or page) 
        - "evidence_doc_name" (str):        Unique Document Identifier of the relevant document containing the evidence
        - "evidence_page_num" (int):        Page number of the evidence text (ZERO-indexed)
        - "evidence_text_full_page" (str):  Full page extract containing the evidence text
 ```

**`financebench_document_information.jsonl`**:
```text
    - doc_name (str)            Unique Document Identifier. Format: {COMPANY}_{PERIOD}_{TYPE}
    - doc_type (str)            Type of the Document: {"10K", "10Q", "8K", "EARNINGS", "10K_ANNUAL"}
    - doc_period (int)          Period of the relevant financial document
    - doc_link (str)            URL of the relevant document
    - company (str)             Company 
    - comany_sector_gics (str)  Company Sector in terms of GICS standard
```

The above two files can be loaded and joined using:
```python
df_questions = pd.read_json("data/financebench_open_source.jsonl", lines=True)
df_meta = pd.read_json("data/financebench_document_information.jsonl", lines=True)
df_full = pd.merge(df_questions, df_meta, on="doc_name")
```

The relevant financial source documents (PDFS) are located in `/pdfs/`

In addition, we provide the human-annotated model completions of the evaluated model configurations in the paper of the open-source dataset in `/results/`

---

**Citation:** If you use our open-source dataset or refer to our result, please use the following citation:
```latex
@misc{islam2023financebench,
      title={FinanceBench: A New Benchmark for Financial Question Answering}, 
      author={Pranab Islam and Anand Kannappan and Douwe Kiela and Rebecca Qian and Nino Scherrer and Bertie Vidgen},
      year={2023},
      eprint={2311.11944},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```
