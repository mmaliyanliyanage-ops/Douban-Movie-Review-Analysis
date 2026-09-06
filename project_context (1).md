# Project 2: 电影影评数据的获取与分析（豆瓣电影评论爬取与分析）

## Goal
Produce a runnable Python project + filled-in Word report (实验3_电影影评数据的获取与分析.docx), Section 3 (实验过程及结果) and Section 4 (实验总结) only — sections 1–2 already written.

## Data status
**No dataset provided** — this project starts from scraping. Pick ONE movie on https://movie.douban.com/ (e.g. a popular one with 500+ reviews, so class imbalance across 1–5 stars isn't too extreme). Kilo Code should scrape short reviews (短评) and, if accessible, long reviews (长评/影评).

⚠️ Douban aggressively rate-limits/blocks scrapers (needs headers, cookies, delays, possibly login for beyond page 1 of long reviews). Plan for: request headers with real User-Agent, `time.sleep` between requests, and a fallback of saving whatever partial data is collected. If live scraping is blocked in the Kilo Code sandbox (no internet / CAPTCHA), fall back to a cached/sample dataset or ask the user to run the scraper locally and re-upload the CSV.

## Folder structure to create
```
proj2_douban/
  src/
    01_scraper.py         # scrapes short reviews (id, user, rating 1-5, date, text)
    02_rating_viz.py       # rating distribution charts
    03_wordfreq_wordcloud.py
    04_sentiment_snownlp.py
    05_sentiment_classification.py  # BoW/TF-IDF/2-gram/LSA/Word2Vec/pretrained-vec + classifier
    06_clustering.py
  data/
    reviews_raw.csv        # scraped: rating, text, date, useful_count
  outputs/
    figures/
    metrics.json
  report/
    template_requirements.md
    report_draft.md
```

## Task 1 — Scraper (report requirement #1)
- Target: `https://movie.douban.com/subject/{id}/comments?start=0&limit=20&status=P`
- Fields: rating (1-5, from star class name), comment text, date, useful count, username (optional, can anonymize).
- Handle pagination, respect rate limits (sleep 1-3s), set User-Agent header.
- Write up in the report: how the crawler works, what anti-scraping obstacles were hit (login walls, rate limits), and how many reviews were collected.

## Task 2 — Rating visualization (#2)
- Bar chart of rating distribution (1–5 stars), % breakdown.

## Task 3 — Word frequency + word cloud (#3)
- Chinese segmentation with `jieba`, remove stopwords, frequency count, `wordcloud` library render (needs a Chinese font, e.g. SimHei/思源黑体).

## Task 4 — Sentiment analysis with SnowNLP (#4)
- Score each review with SnowNLP (0–1 positivity).
- Compare SnowNLP score vs actual star rating (e.g. bucket into positive/neutral/negative both ways, compute agreement/confusion matrix or correlation).
- Discuss discrepancies in report: sarcasm, short text, domain mismatch (SnowNLP trained on e-commerce reviews, not movie reviews), rating != sentiment (e.g. "good movie, sad ending" rated low but positive text).

## Task 5 — Binary sentiment classification (#5)
- Label: 1-2 stars = negative, 4-5 stars = positive, drop 3-star.
- Build sentence vectors with EACH of: BoW, TF-IDF, 2-gram TF-IDF, LSA (TruncatedSVD on TF-IDF), averaged Word2Vec (train own small model via gensim), averaged pretrained word vectors (e.g. Chinese fastText/Tencent embeddings if available, else skip with a note on why).
- Train a classifier (Logistic Regression or SVM) per vectorization method, compare accuracy/F1 across all methods in a table.

## Task 6 — Clustering (#6)
- Take the sentence vectors from Task 5 (pick best-performing method), run KMeans (k=2-5, try elbow/silhouette).
- Inspect top terms per cluster (closest to centroid) and manually interpret what each cluster represents (e.g. plot-focused vs acting-focused vs complaint reviews).

## Report formatting rules (same template as Project 1)
- Cover page, TOC own page, 3-level numbered headings, 宋体小四号 body / 黑体 headings, 1.25 line spacing, first-line indent, figure captions below/table captions above (numbered by chapter), centered page numbers, header = experiment title, every code/figure/table needs explanatory text.

## Deliverables checklist
- [ ] Chosen movie ID/URL documented
- [ ] reviews_raw.csv with ≥300-500 reviews (or documented reason if fewer)
- [ ] Rating distribution chart
- [ ] Word cloud + top-N frequency table
- [ ] SnowNLP vs rating comparison + discussion
- [ ] Comparison table of 6 vectorization methods x classifier metrics
- [ ] Clustering results + interpretation
- [ ] report/report_draft.md in Chinese, ready to paste into docx
- [ ] Final docx assembly done by Claude using docx skill (not Kilo Code)

## Notes for Kilo Code
- Libraries: requests, beautifulsoup4, pandas, jieba, wordcloud, snownlp, scikit-learn, gensim, matplotlib.
- If scraping is blocked in sandbox: write the scraper to be correct and runnable, but also generate a small synthetic/sample review set (clearly labeled as placeholder) so downstream steps (2-6) can be demoed end-to-end; user can rerun with real scraped data locally.
- Keep code commented — it needs to go straight into the report as "含注释的源代码".
- Random seed = 42 everywhere.
