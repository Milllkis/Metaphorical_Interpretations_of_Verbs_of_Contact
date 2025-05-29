### Computational Analysis of Metaphorical Interpretations of Verbs of Contact in Italian and English  

---

#### **Project Overview**  
This repository contains the code and methodology for the bachelor's thesis *"Computer Analysis of Metaphorical Meanings of Verbs of Contact in Italian and English"*. The study investigates metaphorical extensions of physical contact verbs (e.g., *hit*, *touch*, *push*) using computational linguistics techniques. Key steps include:  
1. **Data collection** from Italian and English Twitter corpora.  
2. **Preprocessing** (lemmatization, filtering by verb lists, sentence isolation).  
3. **Thematic modeling** (Dirichlet Multinomial Mixture) to identify dominant topics.  
4. **Contextual embedding creation** using masked language models (UmBERTo for Italian, Sentence-RoBERTa for English).  
5. **Clustering** (consensus of DBSCAN, HDBSCAN, Spectral Clustering) to group metaphorical usages.  

---

#### **Repository Structure**  
| File | Description |  
|------|-------------|  
| `collect_data.ipynb` | Downloads and preprocesses raw Twitter data. Filters sentences containing target verbs. |  
| `analyse_data.ipynb` | Analyzes text length distributions, performs thematic modeling, and filters contexts by length/topic. |  
| `cluster_data.ipynb` | Generates contextual embeddings, reduces dimensionality (UMAP), and clusters metaphorical usages. |  

---

#### **Datasets**  
- **Italian**: [10M Italian Tweets](https://huggingface.co/datasets/pere/italian_tweets_10M) (COVID-19 discourse, 2020–2023).  
- **English**: [100M English Tweets](https://huggingface.co/datasets/entryu43/twitter100m_tweets) (2020–2023, filtered to Topic #1: "human interactions over time").  

**Final Corpus**:  
- Italian: 3,220 contexts (verbs: *toccare*, *colpire*, *spingere*, etc.).  
- English: 4,600 contexts (verbs: *hit*, *push*, *beat*, etc.).  

---

#### **Dependencies**  
Install libraries:  
```bash  
pip install datasets spacy pandas scikit-learn hdbscan umap-learn transformers sentence-transformers matplotlib seaborn tweetopic  
```  

Download spaCy models:  
```bash  
python -m spacy download en_core_web_sm  # English  
python -m spacy download it_core_news_sm  # Italian  
```  

---

#### **Methodology Workflow**  
1. **Data Collection & Preprocessing** (`collect_data.ipynb`):  
   - Extract sentences with **exactly one target verb** (e.g., *toccare*, *push*).  
   - Exclude sentences with multiple target verbs.  
   - Save to Pandas DataFrames.  

2. **Data Analysis** (`analyse_data.ipynb`):  
   - Lemmatization and tokenization with spaCy.  
   - Filter sentences by length (7–23 tokens).  
   - Thematic modeling using DMM (5 topics).  
   - Exclude non-relevant topics (English only).  

3. **Embedding & Clustering** (`cluster_data.ipynb`):  
   - Mask target verbs and generate contextual embeddings.  
     - Italian: **UmBERTo** (weighted average of last 4 layers).  
     - English: **Sentence-RoBERTa** (dynamic layer selection).  
   - Dimensionality reduction: **UMAP** (Italian: `n_components=27`, English: `n_components=27`, `metric=correlation`).  
   - Consensus clustering:  
     - Optimize hyperparameters for DBSCAN, HDBSCAN, Spectral Clustering.  
     - Generate binary co-occurrence matrices and aggregate into consensus clusters.  
   - Visualize clusters using PCA/t-SNE and extract medoids.  

---

#### **Key Results**  
- Metaphorical usages were successfully clustered (e.g., *appoggiare* "support ideas" vs. "lean physically"; *beat* "win in sports" vs. "overcome illness").  
- Literal and metaphorical meanings formed distinct clusters (e.g., *slap* physical action vs. *slap on a t-shirt* metaphorical).  
- Short contexts (<10 tokens) reduced cluster coherence.  

**Visual Examples**:  
- See `cluster_data.ipynb` for cluster visualizations (PCA/t-SNE) and medoid examples.  
- Example cluster for *push*:  
  - Technical: "push software updates".  
  - Political: "push for policy changes".  
  - Economic: "push up inflation".  
