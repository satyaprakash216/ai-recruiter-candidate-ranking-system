# AI Recruiter - Candidate Ranking System

This project ranks candidates for a job description using a simple hybrid scoring system. It is designed as a clean baseline for a hackathon or assignment submission.

## What it does

The system reads:

- a job description text file
- a candidate CSV containing skills, career history, projects, education, platform activity, and behavioral signals

Then it creates a ranked shortlist using:

1. **Semantic similarity** between the full job description and full candidate profile
2. **Skill overlap** between role requirements and candidate skills
3. **Experience fit** compared with the required years of experience
4. **Behavioral/platform signals** like open-source activity, communication, ownership, tests, and collaboration

## Folder structure

```text
ai_recruiter_system/
├── data/
│   ├── job_description.txt
│   └── candidates.csv
├── output/
│   └── ranked_candidates.csv
├── src/
│   └── rank_candidates.py
├── deck/
│   ├── AI_Recruiter_Approach.pptx
│   └── AI_Recruiter_Approach.pdf
├── requirements.txt
└── README.md
```

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate   # Windows
# source .venv/bin/activate   # Mac/Linux

pip install -r requirements.txt
```

## Run

```bash
python src/rank_candidates.py --jd data/job_description.txt --candidates data/candidates.csv --output output/ranked_candidates.csv
```

Optional top 5 only:

```bash
python src/rank_candidates.py --jd data/job_description.txt --candidates data/candidates.csv --output output/ranked_candidates.csv --top-k 5
```

## Input candidate CSV format

Required columns:

```text
candidate_id,name,current_title,years_experience,skills,career_history,projects,education,platform_activity,behavioral_signals
```

## Output format

The output file contains:

```text
rank,candidate_id,name,rank_score,semantic_score,skill_score,experience_score,behavioral_score,matched_skills,missing_skills,recommendation_reason
```

## Why this approach?

Keyword filters miss strong candidates because they only check exact words. This system looks at the full candidate profile and combines meaning, skills, experience, and behavior into one explainable ranking.

## How to improve later

- Replace TF-IDF with Sentence Transformers or OpenAI embeddings
- Add a cross-encoder or LLM judge for top 20 candidates
- Add bias checks and protected-attribute filtering
- Add a recruiter feedback loop to improve ranking over time
- Add a FastAPI endpoint and simple frontend
