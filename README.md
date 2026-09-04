# araseg-shared-task-AUC-DL-Lab
## Zero-Shot Document Categorization via Gemini
To establish document domain distributions for error profiling, we performed zero-shot categorization
using Gemini 3.6 Flash to reconstruct the categorization shown in the Elkholy et al. (2026a) paper.
Documents were processed in batches of 25. Each prompt included the document identifiers, full
document text, and the target categories allowed. For each document, the model was instructed to return:
1. The assigned category from the permitted taxonomy.
2. A concise (1–2 sentence) summary describing the document content.
The generated categories were subsequently used for error analysis and cross-domain stability
evaluation.

The generated categories were subsequently used for error analysis and cross-domain stability evaluation. **The complete document-to-category mapping, along with the corresponding document descriptions, can be found in the `document_mapping.csv` file.**
