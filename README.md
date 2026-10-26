# TalentVerse — Skills & Career Intelligence
CSB312 CA3 Mini Project 2 · Option 9 · FastAPI + SQLite + Pydantic + HTML/CSS/JS

Workflow: **user input → AI extraction → Pydantic validation → FastAPI → SQLite save → frontend confirmation**

## Features
- **Build Profile** – extract skills, education and experience from resume text
- **Career Plan** – skill gaps + week-by-week milestones for a target role
- **Job Match** – compare your profile with a job description (match %, matched / missing skills)
- Empty/short input is rejected (client + server), saved-records history with View / Delete
- Real AI via OpenAI-compatible API when `OPENAI_API_KEY` is set; otherwise a built-in local analyzer (the UI shows which one was used)
- Deployment Link - https://talentverse-3yo4.onrender.com

## Run on Windows (easiest)
1. Extract the ZIP, open the **TalentVerse** folder.
2. Double-click `run.bat`  — or in a terminal inside that folder:
   ```
   python -m pip install -r requirements.txt
   python run.py
   ```
3. Open http://127.0.0.1:8000  (API docs: http://127.0.0.1:8000/docs)

`run.py` fixes its own working folder, so the "No module named 'app'" error cannot happen.
Everything (requirements.txt, app/, static/) is in the same top-level folder — there is no `backend` subfolder.

## Enable real AI (optional)
Copy `.env.example` to `.env` and set `OPENAI_API_KEY=...` (any OpenAI-compatible provider works via `OPENAI_BASE_URL`).
If the AI call fails, the app falls back to the local analyzer and shows a warning.

## Tests
```
python -m pytest -q          # or: python -m unittest discover -s tests -t .
```

## API
| Method | Path | Purpose |
|---|---|---|
| GET | /api/health | status |
| POST | /api/analyze | analyze + validate + save. Body: `{mode, input_text, profile_text?, target_role?}` |
| GET | /api/profiles | list saved records |
| GET | /api/profiles/{id} | one record |
| DELETE | /api/profiles/{id} | delete |

`mode` = `profile` \| `career_plan` \| `job_match` (job_match also needs `profile_text`).

## Deploy (compulsory) — Render, free tier
1. Create a GitHub repo and push this folder's contents (the `.gitignore` already excludes `.env`).
2. Render → **New → Web Service** → pick the repo.
3. Runtime Python · Build: `pip install -r requirements.txt` · Start: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
   (or choose **Blueprint** — `render.yaml` is included).
4. Optional env vars: `OPENAI_API_KEY`, `OPENAI_MODEL`.
5. Put the resulting `https://….onrender.com` URL and screenshots in your report.
Note: free-tier disk is temporary, so saved records can reset on redeploy; fine for a demo.

## Demo order
Profile → Load sample → Analyze (shows "Saved to database") → Analyze with empty box (validation error) → Saved records → Career Plan → Job Match.
