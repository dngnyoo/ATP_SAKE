This repository contains the structured candidate phrases extracted from the Krapivin dataset, specifically curated for our **Structure-Aware Keyphrase Extraction** project.

## 1. File Description
- **File Name**: preprocessed_krapivin.pkl
- **Format**: A list of Python dictionaries (Pickle format).
- **Target Data**: The test split of the Krapivin dataset.

## 2. Data Structure
Each dictionary in the list represents a single document and contains the following keys:

| Key | Description | Use Case |
| :--- | :--- | :--- |
| doc_id | Unique ID from the original dataset | Identification |
| all_phrases | Combined list of all extracted phrases | Baseline TF-IDF calculation |
| title_phrases | Phrases extracted from the Title | Section Weighting ($W_{{section}}$) |
| abstract_phrases | Phrases extracted from the Abstract | Section Weighting ($W_{{section}}$) |
| body_phrases | Phrases extracted from the Body | Positional Weighting ($W_{{pos}}$) |
| ground_truth | Reference keyphrases provided by authors | Evaluation (F1-score) |

> **Note on body_phrases**: The list preserves the **original sequential order** of appearance in the text. You can use the list index to calculate positional scores.

## 3. Preprocessing Details
- **Compound Word Preservation**: Hyphenated terms (e.g., graph-based, multi-agent) are preserved as single units.
- **Section-Specific Filtering**: 
  - **Title/Abstract**: Includes Nouns, Adjectives, and core Action Verbs to capture the main contribution.
  - **Body**: Restricted to Nouns and Adjectives to minimize noise.
- **Noise Cleaning**: Removed LaTeX commands, citations (e.g., [1], et al.), URLs, and academic artifacts (e.g., i-, th-).

## 4. How to Use
1. **Load the data**:

import pickle
with open('preprocessed_krapivin.pkl', 'rb') as f:
    data = pickle.load(f)

1. **Calculate TF-IDF**: Use `all_phrases` to build your vocabulary and compute base scores.
2. **Apply Weights**: Use the structured lists (`title_phrases`, etc.) to multiply the base scores by our proposed weighting factors.
3. **Evaluate**: Compare your top-ranked phrases with `ground_truth`.