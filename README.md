# [facebook/nllb-200-distilled-600M] Fine tuning
## Machine Translation on Low-Resource Languages


| Translation Language     | Dataset Used       | No. of Sentences | Framework Used |
|--------------------------|--------------------|------------------|----------------|
| English -> Kinnauri       | Kinnauri-Pahari    | 20,307           | PyTorch        |
| English -> Kangri         | Kangri_corpus      | 26,785           | PyTorch        |


For this implementation two Tesla T4 GPUs (15GB) were utilized.
Implementation details:

# 1. Dataset Preparation

- The original dataset contains translations from **Hindi -> Kinnauri** and **Hindi -> Kangri**.
- To fine-tune `nllb-200-distilled-600M` to generate translations from **English -> Kinnauri** and **English -> Kangri**, a dataset in the appropriate form is required.
- **[1-data-ingestion](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/1-data-ingestion.ipynb)**: Generates English sentences corresponding to the Hindi sentences present in the original datasets utilizing `nllb-200-distilled-600M` “base”.

# 2. Fine-tuning

The model is fine-tuned on **English -> Kinnauri** and **English -> Kangri** datasets for **15 & 10 epochs** respectively. However, a batch size of 8 is utilized in Kinnauri compared to 6 in Kangri (*Reason: Larger data size in Kangri*).

The following general steps are followed for both **Kinnauri** and **Kangri**:

- The `NllbTokenizer` has predefined language codes as per **Flores-200**.
- Introduced language codes for **Kinnauri (kang_Deva)** and **Kangri (kangri_Deva)** following the naming conventions for **Hindi (hin_Deva)**.
- Resized the embedding layer of the "base" LLM based on the length of the tokenizer (now containing the language codes for Kinnauri or Kangri).
- **NOTE**: The fine-tuning was performed in different notebooks, so the tokenizer utilized for fine-tuning **English -> Kinnauri** is independent of **English -> Kangri**.

### Kinnauri

- **[2.0-english-kinnauri-ft](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/2-english-kinnauri-ft.ipynb)**, **[2.1-english-kinnauri-evaluation](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/2.1-english-kinnauri-evaluation.ipynb)**, **[2.2-english-kinnauri-result](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/2.2-english-kinnauri-result.ipynb)** contain the implementation of **English -> Kinnauri** translations, “sacrebleu” evaluation, and the generated translations compared with the actual translations.

- **Evaluation score:**
  - **Kinnauri**:
    ```json
    {
      'score': 8.435075362674903,
      'counts': [5013, 1690, 689, 323],
      'totals': [16942, 14911, 12889, 10921],
      'precisions': [29.58918663676071, 11.333914559721011, 5.345643572038172, 2.957604614962],
      'bp': 0.9884977801312381,
      'sys_len': 16942,
      'ref_len': 17138
    }
    ```

### Kangri

- **[3.0-english-kangri-ft](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/3.0-english-kangri-ft.ipynb)**, **[3.1-english-kangri-evaluation](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/3.1-english-kangri-evaluation.ipynb)**, **[3.2-english-kangri-result](https://github.com/Bhavjot-Singh03/Nllb-Machine-Translation-on-LRL/blob/main/3.2-english-kangri-results.ipynb)** contain the implementation of **English -> Kangri** translations, “sacrebleu” evaluation, and the generated translations compared with the actual translations.

- **Evaluation score:**
  - **Kangri**:
    ```json
    {
      'score': 2.6536867735646754,
      'counts': [4399, 1153, 386, 159],
      'totals': [32305, 29626, 26960, 24328],
      'precisions': [13.617087138213899, 3.8918517518396003, 1.4317507418397626, 0.6535679052943111],
      'bp': 1.0,
      'sys_len': 32305,
      'ref_len': 29286
    }
    ```

