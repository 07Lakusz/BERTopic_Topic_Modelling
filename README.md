# BERTopic_Topic_Modelling

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![DOI](https://zenodo.org/badge/1050403474.svg)](https://doi.org/10.5281/zenodo.17054318)

A Google Colab pipeline to extract, model, and visualize topics from bibliographic (`.ris`) data using BERTopic. This repository provides an end-to-end workflow for conducting automated thematic analysis on large volumes of scientific literature.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Methodology](#methodology)
- [Requirements](#requirements)
- [How to Use](#how-to-use)
- [Configuration](#configuration)
- [Understanding the Output](#understanding-the-output)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Overview

Analyzing hundreds or thousands of research paper abstracts to identify key themes is a time-consuming and challenging task. This project provides a ready-to-use Google Colab notebook that automates this process. By simply uploading your bibliographic data in `.ris` format (exported from tools like Zotero, EndNote, or Web of Science), you can leverage state-of-the-art NLP models to uncover, interpret, and visualize the underlying topics within your corpus.

This pipeline is designed to be accessible to researchers and analysts with minimal coding experience while remaining highly configurable for advanced users.

## Features

-   **🖥️ Zero Local Setup:** Runs entirely in Google Colab, requiring only a web browser and a Google account.
-   **📚 Direct `.ris` File Support:** Parses bibliographic files to automatically extract abstracts for analysis.
-   **⚙️ End-to-End Pipeline:** Handles everything from data preprocessing and sentence embedding to topic clustering and visualization.
-   **🔧 Highly Configurable:** A centralized `SETTINGS` cell allows for easy tweaking of all model parameters without touching the core code.
-   **📊 Rich Outputs:** Generates a suite of outputs, including:
    -   Interactive HTML topic visualizations.
    -   High-resolution plots of topic keywords.
    -   CSV files containing sentence-level topic assignments and topic summaries.
    -   The trained BERTopic model for reuse.
-   **🤖 State-of-the-Art NLP:** Built on powerful libraries like `Sentence-Transformers`, `UMAP`, `HDBSCAN`, and `BERTopic`.

## Methodology

The notebook follows a systematic, multi-step process to identify topics:

1.  **Data Ingestion & Preprocessing:** Abstracts are extracted from the uploaded `.ris` files. The text is cleaned by removing common stopwords, and then tokenized into individual sentences, which serve as the primary documents for analysis.
2.  **Sentence Embedding:** Each sentence is converted into a high-dimensional numerical vector using a `SentenceTransformer` model (`all-MiniLM-L6-v2` by default). These embeddings capture the semantic meaning of the sentences.
3.  **Dimensionality Reduction:** **UMAP** (Uniform Manifold Approximation and Projection) is used to reduce the dimensionality of the embeddings, making them more suitable for clustering while preserving their topological structure.
4.  **Clustering:** **HDBSCAN** (Hierarchical Density-Based Spatial Clustering of Applications with Noise) is applied to the reduced embeddings to identify dense clusters of semantically similar sentences. Each resulting cluster represents a topic.
5.  **Topic Representation:** A class-based TF-IDF (c-TF-IDF) algorithm is used to extract the most representative keywords for each topic, providing human-interpretable labels.
6.  **Post-processing & Visualization:** The pipeline includes optional steps for merging similar topics and reassigning outlier sentences. Finally, it generates a series of tables and interactive plots to help you explore the results.

## Requirements

-   A Google Account (for using Google Colab).
-   One or more bibliographic files in `.ris` format containing abstracts.

## How to Use

Follow these steps to run your own analysis:

1.  **Open the Notebook:** Upload the `.ipynb` notebook file to your Google Drive, then open it with Google Colab.

2.  **Cell 1: Installation & Imports**
    -   Run this cell to install all required libraries and download the necessary NLTK data.

3.  **Cell 2: Settings Configuration**
    -   Review the parameters in the `SETTINGS` dictionary. You can leave them as default for your first run, or modify them to suit your needs (see [Configuration](#configuration) below).
    -   Run this cell to load the settings.

4.  **Cell 3: Upload Data and Run Analysis**
    -   Run this cell. A file upload button will appear.
    -   Click the button and select the `.ris` file(s) you want to analyze.
    -   The analysis will begin automatically after the files are uploaded. This process may take several minutes.

5.  **Cell 4: Download Results**
    -   Once the analysis is complete, run this final cell.
    -   It will package all the output files (CSVs, plots, model) into a single `bertopic_results.zip` file and trigger an automatic download to your computer.

## Configuration

All key parameters are located in the `SETTINGS` dictionary in **Cell 2**. This allows you to easily experiment with the pipeline. Some of the most important settings to consider are:

-   `EMBEDDING_MODEL_NAME`: Change the sentence transformer model for different languages or performance characteristics.
-   `HDBSCAN_MIN_CLUSTER_SIZE`: This is a critical parameter. Increase it to get fewer, broader topics; decrease it to find more, smaller, and more granular topics.
-   `CV_NGRAM_RANGE`: By default, it considers single words `(1, 1)` and two-word phrases `(1, 2)`. Adjust to find longer keyphrases.
-   `AUTO_REDUCE_TOPICS`: Set to `False` to skip the automatic topic merging step and see the raw output from HDBSCAN.

## Understanding the Output

After running the pipeline, the downloaded `.zip` file will contain the following:

-   `sentence_topics.csv`: A table mapping every sentence from your abstracts to its assigned Topic ID.
-   `topic_summary_table.csv`: A summary of all topics, their size (sentence count), and their top keywords.
-   `topic_frequencies_barchart_interactive.html`: An interactive barchart showing the size of each topic.
-   `topics_2d_visualization.html`: An interactive 2D scatter plot where you can explore the relationships between topics.
-   `topic_hierarchy_visualization.html`: An interactive dendrogram showing how topics can be hierarchically merged.
-   `topic_keywords_id_*.png`: A series of static bar charts visualizing the top keywords for the most prominent topics.
-   `/bertopic_model/`: A directory containing the fully trained and saved BERTopic model for future use.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Acknowledgments

This project is made possible by the incredible work of the teams behind [BERTopic](https://maartengr.github.io/BERTopic/), [Sentence-Transformers](https://www.sbert.net/), and the broader scientific Python ecosystem.
