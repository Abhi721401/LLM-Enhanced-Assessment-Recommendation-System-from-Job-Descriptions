# LLM-Enhanced-Assessment-Recommendation-System-from-Job-Descriptions

This project implements an AI-based recommendation engine that suggests SHL assessments based on hiring queries or job descriptions. The system combines SentenceTransformer embeddings, FAISS vector similarity search, and Gemini LLM-based skill extraction to match job requirements with the most relevant assessments from an SHL assessment catalog.

### Goal of the Project

To automatically recommend the most suitable psychometric and skill assessments for a given hiring query or job description.
Traditional keyword matching fails to understand meaning; this project solves that using semantic search and LLM reasoning.

### How the System Works
# 1.  Dataset : i)shl_catalog.csv[Prepared by web scraping] (List of SHL product assessments with):
assessment name,
URL,
description,
test category.

ii) train_data.csv: Contains labelled training queries and URLs used for evaluation.

iii) test_queries.cs: Used to generate a submission output file.

# 2. Query Cleaning
All incoming hiring queries and catalog text are cleaned with:
lower-casing,
punctuation removal,
whitespace normalization,
alphanumeric formatting,
ensuring consistent embeddings.

# 3. Gemini Intent Extraction
The system uses Google Gemini LLM to extract structured information from queries:
hard skills,
soft skills,
test type keywords.
The extracted text is appended to the original query to strengthen search signals.

# 4.  Embedding Model
The SHL catalog is embedded using: all-MiniLM-L6-v2 (SentenceTransformer)
This converts assessment descriptions into high-dimensional semantic vectors.

# 5. Vector Search (FAISS)
A FAISS inner-product index is created:
embeddings stored in memory,
similarity search performed at query time,
top semantic matches retrieved.
FAISS enables fast & scalable recommendation retrieval.

# 6. Recommendation Logic
The system:
embeds the enriched query,
searches over the FAISS index,
scores similarity,
filters by test category,
balances knowledge & behaviour tests,
returns top-k recommended items.

📈 Model Evaluation
The engine supports Recall@10 scoring:

Mean Recall@K

Recall@K evaluates how many of the relevant assessments are successfully retrieved within the top K recommendations for a given query:

$$
Recall@K = 
\frac{\text{Number of relevant assessments in top K}}
{\text{Total relevant assessments for the query}}
$$

To evaluate the model across all test queries, we compute the average recall:

$$
MeanRecall@K = 
\frac{1}{N} \sum_{i=1}^{N} Recall@K_i
$$

**Where:**
- **K** = number of retrieved results considered  
- **N** = total number of test queries  
- **Recall@Kᵢ** = recall value for the *i-th* query  

This metric indicates how effectively the system retrieves relevant recommendations within the top-K results. Higher values represent better retrieval accuracy.

### Tech Stack 

| Component        | Technology                       |
| ---------------- | -------------------------------- |
| Programming Lang | Python                           |
| Embeddings       | SentenceTransformer MiniLM-L6-v2 |
| Vector Engine    | FAISS (IndexFlatIP)              |
| LLM Layer        | Gemini Flash API                 |
| Evaluation       | Recall@10                        |
| Libraries        | pandas, numpy, sklearn, regex    |


