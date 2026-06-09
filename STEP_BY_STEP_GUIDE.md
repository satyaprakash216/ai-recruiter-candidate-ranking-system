# Step-by-step guide: AI Recruiter Candidate Ranking System

## 1. What we are building

We are building a system that reads a job description and ranks candidates by real role fit. Instead of only checking exact keywords, it looks at the full candidate profile: career history, skills, projects, education, platform activity, and behavioral signals.

## 2. Input files

### Job description

File: `data/job_description.txt`

This contains the role requirements. Example: Python, FastAPI, PostgreSQL, Docker, 2+ years experience, NLP or ranking interest.

### Candidate data

File: `data/candidates.csv`

Each candidate has these columns:

- candidate_id
- name
- current_title
- years_experience
- skills
- career_history
- projects
- education
- platform_activity
- behavioral_signals

## 3. How the system understands the job

The system reads the whole job description as one text block. It also extracts basic structured information like required years of experience and important skills.

## 4. How the system understands candidates

For every candidate, the system joins all important profile fields into one complete profile text. This gives the ranking model more context than a normal keyword filter.

## 5. Scoring method

The final score is calculated using four parts:

```text
final_score = 0.55 semantic_score + 0.25 skill_score + 0.10 experience_score + 0.10 behavioral_score
```

### Semantic score

Compares the meaning of the job description with the meaning of the complete candidate profile using TF-IDF vectors and cosine similarity.

### Skill score

Checks how many required skills from the job description are found in the candidate profile.

### Experience score

Checks whether the candidate's years of experience meet the required years.

### Behavioral score

Looks for positive signals like communication, ownership, testing, mentoring, open-source activity, and product thinking.

## 6. Output file

File: `output/ranked_candidates.csv`

The output includes:

- rank
- candidate_id
- name
- final rank score
- component scores
- matched skills
- missing skills
- recommendation reason

## 7. How to run

Install dependencies:

```bash
pip install -r requirements.txt
```

Run ranking:

```bash
python src/rank_candidates.py --jd data/job_description.txt --candidates data/candidates.csv --output output/ranked_candidates.csv
```

Top 5 only:

```bash
python src/rank_candidates.py --jd data/job_description.txt --candidates data/candidates.csv --output output/ranked_candidates.csv --top-k 5
```

## 8. How to improve the project

After this baseline works, upgrade it like this:

1. Replace TF-IDF with Sentence Transformer embeddings.
2. Store candidate embeddings in a vector database.
3. Retrieve top 50 candidates using vector search.
4. Use an LLM or cross-encoder to rerank only the top 50.
5. Add fairness checks and remove protected attributes from scoring.
6. Add a recruiter feedback loop.
7. Build a FastAPI backend and simple React dashboard.
