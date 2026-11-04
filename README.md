# Scaler - Learner Profiling & Clustering Case Study
### Data Cleaning & EDA | Manual Heuristic Clustering | K-Means & Hierarchical Clustering | Business Insights & Strategy
---


## Quick Overview

Section | Details 
---|---
Business Problem | Scaler wants to profile the best companies and job positions for its learners. By understanding learner salaries, experience, and company tiering, Scaler can tailor its curriculum, manage learner salary expectations, and build strategic partnerships with high-paying companies. 
Objectives | 1. Clean heavily corrupted raw data (45-digit years, shifted columns).
2. Perform manual heuristic clustering (Tier, Class, Designation).

3. Apply Unsupervised Machine Learning (K-Means & Hierarchical).

4. Provide actionable business recommendations for sales and curriculum teams.

Technical Stack | Python (Pandas, NumPy, Scikit-learn, Matplotlib, Seaborn, SciPy), Jupyter Notebook 
Project Features | - Strict domain-knowledge data filtering
- Regex text cleaning and mapping

- KNN Imputation logic

- Label Encoding & Standardization

- Manual Clustering flags (1, 2, 3)

- K-Means clustering (k=4) and Hierarchical dendrogram

- Dataset reduced from 205,843 messy rows to 130,858 clean, valid rows

Start-to-End Pipeline | Data Loading → Aggressive Data Cleaning → EDA → Regex & Merge → KNN Imputation → Encoding & Scaling → Manual Clustering → K-Means & Hierarchical → Model Comparison → Business Recommendations 
Top Strategic Recommendations | - Shift from B2C to B2B corporate upskilling
- Run targeted "salary rescue" campaigns for underpaid learners

- Equip admissions counselors with a data-driven salary benchmarking tool

- Launch high-ticket masterclasses for specialized roles

Future Upgrades & Scaling Plan | - Deploy the manual Tier/Class logic into a live internal dashboard
- Add geographic data (City) to see if location impacts salary brackets

- Build a real-time salary prediction tool for the sales team

## The Big Picture

Scaler is an online tech-versity offering intensive computer science and data science courses. To optimize its sales, marketing, and curriculum, the analytics team needed to understand the exact profile of their learners. 

This project shows that Scaler's core audience is **mid-career professionals (8-12 years of experience)**, not freshers. By applying rigorous data cleaning and manual clustering, we isolated highly underpaid talent segments (Class 3 and Tier 3) who represent the highest intent for upskilling. 

## Business Problem

Scaler's analytics team needed clear answers to:
- Which companies and job roles dominate our learner base?
- How can we segment learners based on their relative salary in their company?
- Can we find "underpaid" talent to target with specific upskilling courses?
- Do unsupervised algorithms (K-Means) naturally group these same segments?

Note: Without this analysis, Scaler risks spending marketing budgets on the wrong demographic and failing to target the learners who need upskilling the most.

## Objectives
- Clean and preprocess a highly corrupted raw dataset.
- Perform manual clustering using domain logic (percentiles).
- Build unsupervised models to check clustering tendency.
- Compare algorithmic clusters with manual business flags.
- Provide concrete recommendations for the sales and curriculum teams.

## Technical Stack

Python Libraries:
- Pandas and NumPy for data manipulation
- Matplotlib and Seaborn for visualization
- Scikit-learn for KNN Imputation, K-Means, and PCA
- SciPy for Hierarchical Clustering (Dendrogram)

Environment:
- Jupyter Notebook
- Markdown for documentation

## Repository Structure
scaler-clustering-case-study/
│
├── README.md                                    # Project overview
├── scaler_hashed_for_students.csv               # Raw dataset
├── scaler_job_positions_mapping VX.csv          # Raw mapping dataset
├── Manual Clustering Example.xlsx               # Raw heuristic example
├── scaler_final_cleaned_v2.csv                  # Cleaned and engineered dataset
├── Scaler_Clustering.ipynb                      # Complete Jupyter Notebook
└── requirements.txt                             # Required Python libraries

## Column Profiling

Column | Type | Description 
---|---|---
company_hash | Text | Anonymized identifier for the current employer 
email_hash | Text | Anonymized PII (unique learner ID) 
orgyear | Integer | Employment start date (Year) 
ctc | Integer | Current CTC (Salary) 
job_position | Text | Job profile in the company 
ctc_updated_year | Integer | Year in which CTC got updated 

# Analysis Steps

## 1. Data Loading & Aggressive Cleaning
*During EDA, we discovered severe data entry anomalies (45-digit years, CTC of 1 rupee, and shifted columns). We applied strict domain filters.*

```python
# Master Fix: Filtering out corrupted rows
df = df[df['job_position'].astype(str).str.contains(r'[a-zA-Z]', na=False)]
df = df[(df['orgyear'] >= 1980) & (df['orgyear'] <= 2024)]
df = df[df['ctc'] >= 50000]
```
**Findings:**
- Original records: 205,843
- Cleaned valid records: 130,858
- Duplicate learners removed (kept most recent).

## 2. Exploratory Data Analysis (EDA)
### Univariate Analysis
- **CTC:** Highly right-skewed. A massive peak around the 10 Lakh mark.
- **Experience:** The majority of learners have 8 to 12 years of experience. There are almost zero freshers.

### Bivariate Analysis
- **Experience vs CTC:** Massive variance. At 10 years of experience, learners earn anywhere from 3 LPA to 30+ LPA.
- **Company & Job Role:** A few anonymized companies dominate the learner pool. Backend and Fullstack engineers make up the vast majority.

## 3. Data Pre-processing
- **Regex & Mapping:** Standardized 767 unique job titles.
- **Imputation:** KNN Imputation logic prepared (though data was already 0 NaNs after cleaning).
- **Encoding & Scaling:** Label Encoding for Company/Job, and StandardScaler for CTC/Experience.

## 4. Manual Clustering (Heuristics)
We calculated the 75th and 50th percentiles to create three flags:
- **Designation Flag:** Compares learner CTC vs peers with the *exact same experience and role* in their company.
- **Class Flag:** Compares learner CTC vs peers in the same *job role* (all experience levels).
- **Tier Flag:** Compares learner CTC vs the *entire company*.

*Business Query Answering:* We successfully extracted Top 10 Tier 1 employees, Bottom 10 Tier 3 employees, and Top 2 positions per company.

## 5. Unsupervised Learning
- **Clustering Tendency:** Hopkins Statistic confirmed the data is highly clusterable.
- **Elbow Method:** The bend was identified at **k=4**.
- **K-Means & Hierarchical:** Both models were fitted. PCA was used to visualize the 4 clusters in 2D.
- **Model Comparison:** K-Means grouped learners primarily by company and role, rather than salary. This proved that manual heuristics (Tier/Class) are actually more useful for finding salary outliers than unsupervised clustering.

## 6. Top Insights

- **The "Mid-Career" Sweet Spot:** Scaler's core demographic is mid-to-senior professionals (8-12 years). There are very few freshers.
- **Extreme Salary Inequality:** Years of experience do not guarantee high pay. Salary is highly dependent on the specific company and role.
- **The "Underpaid Data Scientist" Anomaly:** Manual Class flags revealed that many Data Scientists are severely underpaid compared to their peers.
- **Manual > Unsupervised:** For salary segmentation, manual percentile flags are more accurate than K-Means because K-Means gets distracted by categorical variables (like company names).

## 7. Business Recommendations

Priority | Recommendation | Expected Impact 
---|---|---
1 | Shift from B2C to B2B Corporate Upskilling | Highest 
2 | Targeted "Salary Rescue" Campaigns (Target Class 3) | High 
3 | Data-Driven Admissions Counseling (Show peers' salaries) | Medium-High 
4 | Launch High-Ticket Masterclasses for specialized niches | Medium 

## Future Upgrades & Scaling Plan

- **Deploy a Benchmarking Dashboard:** Turn the manual Tier/Class logic into a live internal tool for the sales team.
- **Add more data sources:** Include geographic data (City) to see if location impacts salary brackets.
- **Build a Salary Predictor:** Create a real-time API where a learner inputs their profile and gets their predicted market rate.

## How to Use This Repository

- Clone the repo:
```bash
git clone https://github.com/your-username/scaler-clustering-case-study.git
```

- Install required libraries:
```bash
pip install -r requirements.txt
```

- Open the Jupyter Notebook:
```bash
jupyter notebook Scaler_Clustering.ipynb
```

- Run the cells in sequence.
- Review the outputs and visualizations.

## 👤 Author

### Shaik Mayeenuddin

#### Business Analyst | Data Analytics & AI/ML | Optimizing Processes to Drive Revenue & Retention
