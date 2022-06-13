# Apache Beam Prediction Demo

These notebooks demonstrate how Apache Beam can be used to create a pipeline for predicting the topics of incoming text. The project is broken down into two components:
- `topic-model.ipynb`: a notebook to train the topic model using sentence transformer and clustering
- `topic-pipeline.ipynb`: a notebook to define and run the pipeline

The topic model is trained on the complete works of Shakespeare available from [Gutenberg](https://www.gutenberg.org/ebooks/100) and the pipeline is then applied to the text of King Lear (downloaded from Google Cloud Storage at `gs://dataflow-samples/shakespeare/kinglear.txt`)

## Topic Model

The topic model uses a sentence transformer ([Universal Sentence Encoder](https://tfhub.dev/google/universal-sentence-encoder/4)) to generate embedding vectors for each line of text. These embeddings can be used to determine the semantic similarity between strings. The simplistic approach in this example is to use a k-means clustering model on the embeddings where each cluster represents a different topic. This model is then exported as a `.joblib` file to be used in the pipeline.

## Pipeline

The pipeline defines [ParDo functions](https://beam.apache.org/documentation/programming-guide/#pardo) for the embedding and prediction steps and then runs them locally. The embedding step uses the same sentence transformer and the prediction step uses the `.joblib` file created by the topic model.