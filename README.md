# Hack2Skill INDIA RUNS - Candidate Ranking System

## Project Overview

This project builds an **AI-powered candidate ranking system** for hiring a Senior AI Engineer at Redrob. The system analyzes 100,000 candidate profiles and identifies the top 100 best-fit candidates based on experience, technical skills, and behavioral signals.

---

## Problem Statement

Recruiters often miss the right talent despite extensive searching. Traditional keyword-matching approaches fail to understand:
- **Real experience** vs. keyword stuffing
- **Relevant skills** in context
- **Candidate engagement** and availability
- **Career trajectory** and growth

This project solves this by building an intelligent ranking system that evaluates candidates holistically.

---
## Data

### **Source Data**
- Original file: `candidates.jsonl` (100,000 candidate profiles)
- Provided by: Hack2Skill INDIA RUNS Challenge
- Size: 465MB

### **Processed Data**
- File: `candidates_simple.csv` (included in repo)
- Records: 100,000 candidates
- Features: 28 extracted signals
- Size: ~30MB

To run the code:
1. Download `candidates.jsonl` from Hack2Skill
2. Run the Jupyter notebook
3. It will generate processed CSV and rankings

## Solution Approach

### **Data Processing**
1. **Convert JSONL to CSV**: Transformed 100,000 candidate profiles from JSONL format to tabular CSV
2. **Feature Extraction**: Extracted 28 key features including:
   - Experience metrics (years, AI/ML-specific years)
   - Technical signals (ranking/retrieval systems, embeddings, vector DBs)
   - Behavioral signals (recruiter response rate, activity, notice period)
   - Location and relocation willingness

### **Quality Filtering**
- **Title-based filtering**: Kept only candidates with AI/ML-related job titles
- **Result**: 100,000 candidates → 1,939 with relevant titles (1.94%)
- This eliminates "trap" candidates (Marketing Managers, Business Analysts pretending to be engineers)

### **Scoring Formula**
FINAL_SCORE = (15% × Experience Score) +

(15% × AI/ML Years Score) +

(25% × Ranking/Retrieval Score) +

(20% × Tech Stack Score) +

(15% × Behavioral Score) +

(10% × Location Score)
#### **Score Components:**

1. **Experience Score (15%)**
   - Target: 5-9 years total experience
   - Scoring: 100 if in range, scaled otherwise

2. **AI/ML Years Score (15%)**
   - Measures actual AI/ML experience (not just career switching)
   - Target: 5+ years in AI/ML roles
   - Scoring: 100 if 5+ years, scaled down otherwise

3. **Ranking/Retrieval Score (25%)** ⭐ **MOST IMPORTANT**
   - Binary: Did they build ranking/retrieval/search systems?
   - This is the core job requirement
   - Scoring: 100 (yes) or 0 (no)

4. **Tech Stack Score (20%)**
   - Binary: Do they have vector DB OR embeddings experience?
   - Technologies: Pinecone, Weaviate, Qdrant, Milvus, FAISS
   - Scoring: 100 (yes) or 0 (no)

5. **Behavioral Score (15%)**
   - Recruiter response rate (0-1 converted to 0-100)
   - Higher = more engaged, easier to hire
   - Average in dataset: 0.49

6. **Location Score (10%)**
   - 100 if in India (preferred location)
   - 80 if willing to relocate
   - 20 otherwise

---

## Data Insights

### **Candidate Distribution**
- **Total candidates**: 100,000
- **With 5-9 years experience**: 34,375 (34.4%)
- **With ranking/retrieval experience**: 10,672 (31% of 5-9yr group)
- **With ranking + tech stack**: 1,721 (16.1%)
- **With ranking + tech + high engagement**: 850 (49.4%)
- **Recently active (< 3 months)**: 390 (45.9%)
- **With AI/ML job titles**: 1,939 (1.94%)

### **Key Findings**
- 98.06% of candidates have non-AI/ML titles = **many trap candidates**
- Only 1.94% are actually AI/ML professionals
- Top candidates cluster in India (Bangalore, Delhi, Gurgaon)
- Companies represented: Microsoft, BYJU'S, Swiggy, CRED, Zoho, etc.

---

## Top 10 Results

| Rank | Candidate ID | Score | Key Signals |
|------|--------------|-------|-------------|
| 1 | CAND_0010603 | 99.10 | 5.3 yrs AI/ML \| Ranking + Embeddings \| 94% engagement |
| 2 | CAND_0030031 | 99.10 | 5.7 yrs AI/ML \| Ranking + Vector DB + Embeddings \| 94% |
| 3 | CAND_0061265 | 99.10 | 6.6 yrs AI/ML \| Ranking + Vector DB + Embeddings \| 94% |
| 4 | CAND_0064326 | 99.10 | 7.6 yrs AI/ML \| Ranking + Vector DB + Embeddings \| 94% |
| 5 | CAND_0070525 | 98.95 | 5.4 yrs AI/ML \| Ranking + Embeddings \| 93% |

---

## Files in This Repository
---

## How to Run the Code

### **Prerequisites**
```bash
pip install pandas numpy jupyter
```

### **Step 1: Data Preparation**
Run the data conversion section in the notebook to convert JSONL to CSV:
```python
# Loads candidates.jsonl (100,000 candidates)
# Extracts 28 features
# Outputs: candidates_final.csv
```

### **Step 2: Scoring & Ranking**
Run the scoring section to rank candidates:
```python
# Applies scoring formula to all candidates
# Filters for AI/ML titles only
# Outputs: top_100_ranked_filtered.csv
```

### **Step 3: Generate Final Submission**
Run the final submission section:
```python
# Adds reasoning for each candidate
# Formats as required
# Outputs: final_submission.csv
```

---

## Methodology & Rationale

### **Why This Approach?**

1. **Holistic Evaluation**
   - Not just keywords, but context and skills
   - Considers engagement and availability
   - Validates experience through career history

2. **Transparent Scoring**
   - Each weight is justifiable
   - Formula can be audited
   - Easy to adjust if needed

3. **Trap Detection**
   - Filters by job title (eliminates non-engineers)
   - Checks career history (validates claims)
   - Uses multiple signals (not just one metric)

4. **Scalability**
   - Processes 100,000 candidates in seconds
   - Can be re-run with new data
   - Weights can be adjusted

### **Why These Weights?**

- **Ranking/Retrieval (25%)**: Core job requirement
- **Experience (15%)**: Shows seniority
- **AI/ML Years (15%)**: Proves deep expertise
- **Tech Stack (20%)**: Modern skills needed
- **Behavioral (15%)**: Can actually be hired
- **Location (10%)**: Logistical bonus

---

## Key Insights

1. **Most candidates are not AI/ML engineers** (98% have other titles)
2. **Ranking/retrieval experience is rare** (only 31% of target experience group)
3. **Vector DB + Embeddings combo is even rarer** (only 16%)
4. **Behavioral engagement matters** (49% of qualified candidates are responsive)
5. **Top candidates cluster in Tier-1 Indian cities** (Bangalore, Delhi, Gurgaon)

---

## Future Enhancements

1. **LLM-based evaluation** - Use Claude/GPT to understand career descriptions
2. **Graph analysis** - Analyze candidate connections and company networks
3. **Skill depth analysis** - Quantify years spent on each specific skill
4. **A/B testing** - Compare different weighting schemes
5. **Continuous learning** - Update weights based on hiring outcomes

---

## Technical Stack

- **Python 3.x**
- **Pandas** - Data manipulation and analysis
- **Jupyter Notebook** - Interactive development and documentation

---

## Contact & Questions

For questions about this project, contact: [abhishekjadhao09@gmail.com]

---

## License

This project is part of Hack2Skill INDIA RUNS challenge.