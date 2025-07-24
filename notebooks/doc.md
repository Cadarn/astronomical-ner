# Data Annotation for Astronomical NER

## Objective

The goal of this notebook is to process raw NASA ADS abstract data into a structured format suitable for training a spaCy Named Entity Recognition (NER) model. This involves identifying astronomical source names (like "SN 2023ixf" or "Crab Nebula") in the text and marking their exact locations.

## Process Overview

The process consists of the following key steps:

1.  **Load Raw Data**: Ingest the downloaded JSON files containing abstracts and metadata from NASA ADS.
2.  **Extract Gold-Standard Entities**: Use the `keywords` field in the data to identify "gold-standard" astronomical entities. We specifically look for keywords prefixed with `individual:`, which reliably tag specific celestial objects.
3.  **Locate Entities in Text**: Search for these extracted entities within the corresponding document's `title` and `abstract`.
4.  **Format for spaCy**: Structure the text and entity locations into the specific format required by spaCy for training, which is a list of tuples, where each tuple contains the text and a dictionary of entity spans.

## Key Functions

-   `extract_astronomical_keyword_entities()`: Parses the `keyword` list from a document to pull out any terms identified as an "individual" celestial object.
-   `find_exact_matches()`: A robust function that uses regular expressions with word boundaries (`\b`) to find the precise start and end character offsets of an entity string in a body of text. This prevents partial matches (e.g., finding "Norma" inside "34 Normae").
-   `build_data_model()`: The main orchestration function that takes a raw document, runs the extraction and search steps, and compiles the final structured output.
-   `build_spacy_ner_data()`: Formats the final text and entity locations into the `(text, {"entities": [...]})` tuple structure that spaCy expects for a single training example.

## Final Output

The primary output of this notebook is a list of spaCy training examples stored in the `spacy_ner_data` key of the generated data model. This data can be saved and used directly in a spaCy training pipeline to teach the model to recognize astronomical objects.
