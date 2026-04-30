# From Raw Text to a Domain Knowledge Graph

## Overview

This case study implements an NLP pipeline for constructing a knowledge graph (KG) from movie plot summaries.

The pipeline includes:
- entity extraction
- relation extraction
- triple normalization
- graph construction
- comparison of extraction strategies
- manual precision evaluation

## Dataset

- https://huggingface.co/datasets/vishnupriyavr/wiki-movie-plots-with-summaries

A filtered sample of 250 movie plots was used.

## Methods

### 1. Rule-based extraction
- Entities: capitalized name patterns
- Relations: predefined templates (e.g. "X kills Y", "X meets Y")

### 2. Off-the-shelf NLP extraction
- Based on dependency parsing
- Subject–Verb–Object (SVO) extraction using textacy

## Results

### Coverage

| Strategy | Raw Extractions | Unique Triples | Unique Entities | Relation Types |
|---|---:|---:|---:|---:|
| Rule-based regex | 35 | 35 | 64 | 9 |
| Off-the-shelf textacy SVO | 2449 | 2439 | 2589 | 1091 |

### Manual Precision Audit

| Strategy | Audited | Correct | Precision |
|---|---:|---:|---:|
| Rule-based regex | 10 | 9 | 90% |
| Off-the-shelf textacy SVO | 10 | 6 | 60% |

### Graph Structure

| Strategy | Nodes | Edges | Weak Components | Largest Component | Density |
|---|---:|---:|---:|---:|---:|
| Rule-based regex | 64 | 35 | 29 | 4 | 0.008681 |
| Off-the-shelf textacy SVO | 2589 | 2439 | 482 | 1414 | 0.000338 |

## Structural Insights

- The rule-based graph is small and highly fragmented, consisting mostly of tiny disconnected components.
- The off-the-shelf graph is much larger and contains a substantial connected component (1414 nodes).
- However, the off-the-shelf graph has extremely low density, meaning that connections between entities are weak and sparse.
- This confirms that larger extraction does not necessarily produce a more structured or meaningful graph.

## Key Finding

The experiment shows a precision–coverage trade-off. Rule-based extraction is much smaller but more reliable, while off-the-shelf SVO extraction has far broader coverage but lower precision and noisier relation labels.

## Key Findings

- Rule-based extraction is highly precise (90%) but produces fewer triples.
- Off-the-shelf extraction provides broader coverage but introduces noise (60% precision).
- Off-the-shelf relations are more diverse but less controlled.
- Knowledge graphs built from movie plots are sparse and fragmented.

## Error Analysis

Common errors in off-the-shelf extraction:
- Generic subjects: "information", "both", "parts"
- Incorrect object boundaries: "to shoot him"
- Weak or meaningless relations

Example:
- ("information", "enables", "Dorval accomplice")
- ("both", "wear", "hat")

## Limitations

- Small dataset sample
- No coreference resolution
- Limited relation normalization
- Simplified extraction methods
- Graphs remain sparse due to narrative structure
