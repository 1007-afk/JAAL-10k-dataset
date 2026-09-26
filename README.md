# JAAL-10k

## Dataset Card

**JAAL-10k: A Synthetic Hinglish Scam-Call Conversation Dataset**

### 1. DATASET SUMMARY

**Dataset name:**
JAAL-10k

**Task:**
Multi-class scam-call conversation classification

**Domain:**
Conversational fraud / scam detection

**Language:**
Hinglish (Hindi-English code-switched conversational text)

**Number of synthetic conversations:**
9,677

**Number of scam categories:**
8

**External evaluation set:**
77 independently sourced YouTube scam conversations

**Primary use:**
Research on fine-grained scam-category classification, multilingual/code-switched conversational modeling, synthetic-data evaluation, and synthetic-to-external domain generalization.

---

### 2. MOTIVATION

JAAL-10k was developed to study conversational scam classification in Hinglish, a commonly observed Hindi-English code-switched communication setting. The dataset focuses on fine-grained scam categories rather than binary scam/non-scam detection.

The dataset is intended to support controlled experimentation with synthetic conversational data and evaluation of whether models trained on such data generalize to independently sourced conversations.

---

### 3. DATASET COMPOSITION

The synthetic corpus contains 9,677 multi-turn conversations distributed across eight scam categories:

1. **Banking / KYC / OTP Fraud:** 1,310
2. **UPI / Wallet Fraud:** 949
3. **Investment / Task Scam:** 796
4. **Digital Arrest / Government Impersonation:** 971
5. **Loan / Credit App Scam:** 928
6. **Delivery / Customer Care Scam:** 854
7. **Dating / Romance / Sextortion:** 900
8. **Legacy Telecom Scam:** 2,969

An additional 77 YouTube conversations are maintained separately as an external evaluation set and are not included in the synthetic training, validation, or test partitions.

---

### 4. DATA GENERATION

The synthetic conversations were generated from approximately 600 scam-related seed/context ideas derived from a Kaggle source corpus.

Generation used a dual-agent conversational setup based on Gemma 3 27B. A caller agent and a receiver agent interacted over multiple turns.

The caller was conditioned on the supplied scam context, while the receiver generated responses based on the conversation history. The resulting conversations were written as multi-turn Hinglish interactions.

The generation process was designed to produce conversational scam scenarios rather than isolated scam messages.

---

### 5. LABELING AND CATEGORY ASSIGNMENT

Following generation, conversations were categorized using a lexical/rule-based procedure. The resulting assignments were subsequently reviewed using Gemma-based classification.

The Gemma-based review was used to verify assigned categories and reclassify conversations when appropriate.

The dataset was subsequently upscaled to increase representation of underrepresented categories while retaining Legacy Telecom as the largest category.

---

### 6. DATA SPLITS

The 9,677 synthetic conversations were divided using a stratified 70/15/15 train/validation/test split.

- **Train:** 6,773
- **Validation:** 1,452
- **Test:** 1,452

#### Per-class split:

| Class | Train | Validation | Test |
| :--- | :---: | :---: | :---: |
| Banking / KYC / OTP | 916 | 197 | 197 |
| UPI / Wallet | 665 | 142 | 142 |
| Investment / Task | 557 | 120 | 119 |
| Digital Arrest / Government | 679 | 146 | 146 |
| Loan / Credit App | 650 | 139 | 139 |
| Delivery / Customer Care | 598 | 128 | 128 |
| Dating / Romance / Sextortion | 630 | 135 | 135 |
| Legacy Telecom | 2,078 | 445 | 446 |

The YouTube set is held out entirely from these splits.

---

### 7. CONVERSATION CHARACTERISTICS

Across the 9,677 synthetic conversations:

- **Total word-level token occurrences:** 3,091,317
- **Mean word-level tokens per conversation:** 319.45
- **Median word-level tokens:** 273
- **Minimum:** 10
- **Maximum:** 2,282
- **Mean characters per conversation:** 1,875.61
- **Median characters per conversation:** 1,618
- **Mean vocabulary size per conversation:** 161.90
- **Median vocabulary size:** 155

**Conversation length categories:**
- **Short conversations:** 2,450
- **Medium-length conversations:** 4,821
- **Long conversations:** 2,406

---

### 8. LANGUAGE CHARACTERISTICS

The synthetic train, validation, and test partitions show broadly similar language composition.

| Split | English | Hindi | Ambiguous | Other |
| :--- | :---: | :---: | :---: | :---: |
| Train | 52.64% | 42.30% | 0.30% | 4.76% |
| Validation | 50.46% | 44.50% | 0.23% | 4.81% |
| Test | 51.79% | 42.23% | 0.56% | 5.42% |

**Conversation-level language categories (Training set):**
- Hinglish mixed: 69.98%
- English dominant: 29.44%
- Hindi dominant: 0.44%
- Other dominant: 0.13%

**External YouTube set (Hindi-heavy):**
- English: 11.84%
- Hindi: 82.43%
- Ambiguous: 2.73%
- Other: 3.00%
- *Conversation level:* 57.14% Hinglish mixed, 42.86% Hindi dominant.

---

### 9. VOCABULARY AND OOV CHARACTERISTICS

A word-level tokenizer using the pattern `[A-Za-z0-9]+(?:['’-][A-Za-z0-9]+)?` was used for lexical analysis.

**Relative to the training vocabulary:**

| Dataset | Unique Tokens | Unique OOV Tokens | OOV Rate | Mean Conv OOV Rate | Train Overlap |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Synthetic Test | 10,799 | 1,836 | 17.00% | 0.63% | 99.21% |
| YouTube | 7,133 | 3,608 | 50.58% | 6.56% | 85.49% |

*Note: These are lexical word-level OOV rates, not BPE/subword <UNK> rates.*

---

### 10. SUBWORD TOKENIZATION

Subword analysis was performed with `FacebookAI/roberta-base` and `answerdotai/ModernBERT-base`.

| Model | Synthetic Train (mean) | Synthetic Test (mean) | YouTube (mean) |
| :--- | :---: | :---: | :---: |
| RoBERTa | 607.05 | 610.32 | 3,309.87 |
| ModernBERT | 595.41 | 597.81 | 2,818.62 |

- Neither tokenizer produced explicit UNK tokens in the analyzed data.
- YouTube conversations show substantially greater subword fragmentation.
- ModernBERT produced fewer raw encoder tokens on average than RoBERTa.
- Max sequence lengths: RoBERTa (512), ModernBERT (2048).

---

### 11. DUPLICATE AND SIMILARITY ANALYSIS

- **Exact duplicates:** None identified in the synthetic corpus.
- **Normalized duplicates:** None identified.
- **Near-duplicates (TF-IDF cosine similarity > 0.90):**
  - Pairs: 11
  - Cross-class pairs: 2
  - Clusters: 6 (Largest size: 3)
- **Train/Test overlap:** No exact or normalized matches; 3 highly similar pairs (manual inspection shows template-level overlap).

---

### 12. EXTERNAL YOUTUBE EVALUATION SET

Contains 77 independently sourced YouTube scam conversations.

**Class supports:**
- Banking / KYC / OTP: 17
- UPI / Wallet: 9
- Investment / Task: 12
- Digital Arrest / Government: 5
- Loan / Credit App: 2
- Delivery / Customer Care: 6
- Dating / Romance / Sextortion: 5
- Legacy Telecom: 21

**Characteristics:**
- Substantially longer: Mean word-level tokens (1,378.03), Median (915), Max (18,381).
- Higher lexical novelty, greater subword fragmentation, and different language distribution.
- *Note: Results on this subset should be interpreted as an external stress test.*

---

### 13. HUMAN VALIDATION

Human validation covered 968 synthetic conversations (~10%) and the 77 external YouTube conversations.

**Synthetic Validation:**
- Annotator 1: Accuracy 86.16%, Macro-F1 0.8647
- Annotator 2: Accuracy 76.55%, Macro-F1 0.7619
- Inter-annotator agreement: 76.86% (Cohen's kappa: 0.7207)

**Overall (including YouTube):**
- Annotator 1: Accuracy 84.78%, Macro-F1 0.8484
- Annotator 2: Accuracy 74.83%, Macro-F1 0.7451
- Inter-annotator agreement: 75.31% (Cohen's kappa: 0.7014)

**YouTube Validation:**
- Annotator 1: Accuracy 67.53%, Macro-F1 0.6168
- Annotator 2: Accuracy 53.25%, Macro-F1 0.4859
- Inter-annotator agreement: 55.84% (Cohen's kappa: 0.4484)

---

### 14. BENCHMARK RESULTS

**Synthetic Test Set:**

| Model | Accuracy | Macro-F1 |
| :--- | :---: | :---: |
| RoBERTa-base | 92.63% | 92.33% |
| ModernBERT-base | 92.49% | 92.08% |
| Linear SVM | 90.98% | 90.65% |
| Logistic Regression | 90.56% | 90.27% |
| Transformer Encoder | 89.53% | 89.28% |

**External YouTube Set:**

| Model | Accuracy | Macro-F1 |
| :--- | :---: | :---: |
| RoBERTa-base | 59.74% | 50.17% |
| ModernBERT-base | 61.04% | 56.18% |
| Linear SVM | 49.35% | 39.65% |
| Logistic Regression | 44.16% | 26.56% |
| Transformer Encoder | 49.35% | 44.45% |

---

### 15. ZERO-SHOT FOUNDATION MODEL EVALUATION

**GPT-OSS 120B:**
- Synthetic test: Accuracy 83.06%, Macro-F1 83.32%
- YouTube: Accuracy 66.23%, Macro-F1 63.75%

**Gemma 4 31B:**
- Synthetic test: Accuracy 86.85%, Macro-F1 86.79%
- YouTube: Accuracy 74.03%, Macro-F1 68.61%

---

 ## Video ID Extraction and Source Provenance

  Each file in the YouTube set retains its official 11-character YouTube video ID directly within its filename, encoded after a double underscore separator:

  `[scam_topic]__[VIDEO_ID].txt`
  *(e.g., `bank_impersonation__0lZwAXgaFu4.txt` -> Video ID: `0lZwAXgaFu4`)*

  To access or verify the original source video, extract the trailing 11-character identifier and append it to the standard YouTube URL:

  https://www.youtube.com/watch?v=<VIDEO_ID>



