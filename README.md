# Douban Movie Reviews Analysis

Chinese-language NLP and sentiment analysis on 1M+ user reviews scraped
from Douban (China's largest film review platform). The project moves from
exploratory analysis through sentiment scoring, text vectorization,
classification, and unsupervised clustering of review content.

## Dataset

- **Source:** `DMSC.csv` — Douban Movie Short Comments (~1,048,575 reviews)
- **Fields:** `Star` (1–5 user rating), `Comment` (review text), `Like`
  (popularity), `Date`, movie identifiers
- Text-heavy steps run on a representative 20,000-row sample to keep
  runtime manageable.

## What's inside

1. **EDA & Visualization** — rating distribution, ratings over time,
   review length stats, Chinese word clouds, top-word frequency (jieba
   tokenization + stopword filtering).
2. **Sentiment Analysis (SnowNLP)** — compute a [0,1] sentiment score per
   review and compare it against actual star ratings (Pearson correlation,
   agreement rate, confusion matrix). Includes a discussion of sentiment
   analysis limitations (sarcasm, domain slang, granularity mismatch).
3. **Text Preprocessing** — jieba segmentation, stopword removal, tokenized
   corpus construction; binary label creation (negative 1–2★ vs positive
   4–5★, excluding neutral 3★).
4. **Feature Engineering** — six vector representations compared:
   Bag-of-Words, TF-IDF (1-gram), TF-IDF (2-gram), LSA (TruncatedSVD on
   TF-IDF), and a lightweight Word2Vec-style mean embedding.
5. **Binary Classification** — Logistic Regression trained on each
   representation; evaluated with accuracy, F1, and ROC-AUC to compare
   which vectorization best separates sentiment.
6. **Cluster Analysis** — K-Means on TF-IDF vectors (k selected via
   silhouette score), with top-term inspection per cluster and PCA
   visualization to interpret topical themes (e.g. plot, acting, visuals).
7. **Conclusion & Future Work** — key findings and next steps (e.g.
   pretrained Chinese embeddings, deep learning classifiers).

## Key findings

- Ratings skew positive (concentrated at 4–5 stars), a common pattern on
  review platforms.
- SnowNLP sentiment correlates with star rating only moderately — sarcasm
  and Douban-specific slang limit accuracy.
- TF-IDF and its LSA projection outperform BoW and n-grams for
  classification; Word2Vec mean is competitive but data-limited.
- Clusters separate by **topic** (plot vs. acting vs. visuals), not by
  sentiment — vocabulary reflects what a review is about more than how
  positive it is.

## Tech stack

Python · pandas · numpy · matplotlib · seaborn · jieba · SnowNLP ·
scikit-learn · scipy · WordCloud

## Usage

```bash
pip install pandas numpy matplotlib seaborn jieba snownlp scikit-learn scipy wordcloud jupyter
jupyter notebook douban_movie_analysis.ipynb
```

Place `DMSC.csv` in a `../data/` folder relative to the notebook (or update
the load path in Section 1).

## Data source

Douban Movie Short Comments dataset — commonly available on Kaggle
(search "DMSC Douban Movie Short Comments dataset").
