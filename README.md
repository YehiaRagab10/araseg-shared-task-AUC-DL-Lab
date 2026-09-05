# araseg-shared-task-AUC-DL-Lab
## Zero-Shot Document Categorization via Gemini
To establish document domain distributions for error profiling, we performed zero-shot categorization
using Gemini 3.6 Flash to reconstruct the categorization shown in the [Elkholy et al. (2026a)](https://arxiv.org/pdf/2606.08025) paper.
Documents were processed in batches of 25. Each prompt included the document identifiers, full
document text, and the target categories allowed. For each document, the model was instructed to return:
1. The assigned category from the permitted taxonomy.
2. A concise (1–2 sentence) summary describing the document content.
The generated categories were subsequently used for error analysis and cross-domain stability
evaluation.

The generated categories were subsequently used for error analysis and cross-domain stability evaluation. **The complete document-to-category mapping, along with the corresponding document descriptions, can be found in the [`Documentation/document_mapping.csv`](Documentation/document_mapping.csv) file.**

## Training Hyperparameters

The complete hyperparameter configurations used for all models are provided as JSON files in [`Documentation/Hyperparameters/`](Documentation/Hyperparameters/). These files contain the configurations used for training and reproducing the systems described in the paper.

The available configurations are:

- [`bert_soups_closed.json`](Documentation/Hyperparameters/bert_soups_closed.json): Hyperparameter configurations for the three BERT model soups used in the closed track.
- [`bert_soups_open.json`](Documentation/Hyperparameters/bert_soups_open.json): Hyperparameter configurations for the three BERT model soups used in the open track, including external-data settings.
- [`hdp_graph.json`](Documentation/Hyperparameters/hdp_graph.json): Architecture and training configuration for the HDP_Graph model.
- [`camelbert_araelectra.json`](Documentation/Hyperparameters/camelbert_araelectra.json): Shared architecture and training configuration for the CAMeLBERT-MSA and AraELECTRA models.
- [`qwen_jpt_qlora.json`](Documentation/Hyperparameters/qwen_jpt_qlora.json): Hyperparameter configuration for the Qwen3-4B + JPT + QLoRA approach.

These files provide the exact model, optimization, loss, augmentation, data-selection, and inference configurations used in the reported experiments.

## Prompt Templates
The full prompt templates used for the zero-shot baselines and the QLoRA supervised fine-tuning (SFT) setups are documented in [`Documentation/prompt_templates.md`](Documentation/prompt_templates.md). This includes the QLoRA SFT delimiter-insertion prompt, and both versions (v1 and v2) of the zero-shot generation baseline prompt used for sentence-unit boundary detection.
