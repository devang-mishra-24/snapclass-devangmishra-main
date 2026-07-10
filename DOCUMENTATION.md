# SnapClass (ai-attendance-project-app)

## Project overview

SnapClass is a Streamlit application that provides AI-assisted attendance for classrooms using face and voice recognition. It uses Supabase as the backend database and includes pipelines for face embeddings (dlib + face_recognition_models) and voice embeddings (Resemblyzer + librosa). Teachers can create subjects and take attendance via photos or voice; students can register and enroll.

## Quick start

- Install dependencies:

```bash
pip install -r requirements.txt
```

- Configure Supabase credentials in `.streamlit/secrets.toml`:

```toml
SUPABASE_URL = "your_supabase_url"
SUPABASE_KEY = "your_supabase_anon_or_service_key"
```

- Run the app:

```bash
streamlit run app.py
```

## High-level structure

- [app.py](app.py) — Streamlit entrypoint and page routing (home / teacher / student).
- [requirements.txt](requirements.txt) — Python dependencies.

### src/

- src/components/* — UI components and dialogs
  - [src/components/header.py](src/components/header.py) — Header renderers for home and dashboard.
  - [src/components/footer.py](src/components/footer.py) — Footer renderers.
  - [src/components/subject_card.py](src/components/subject_card.py) — Card used to display subject info.
  - [src/components/dialog_auto_enroll.py](src/components/dialog_auto_enroll.py) — Dialog to auto-enroll students via join-code.
  - [src/components/dialog_add_photo.py](src/components/dialog_add_photo.py) — (UI) dialog to add photos for attendance (used by teacher).
  - [src/components/dialog_enroll.py](src/components/dialog_enroll.py) — (UI) enroll dialog for students.
  - [src/components/dialog_create_subject.py](src/components/dialog_create_subject.py) — Subject creation dialog.
  - [src/components/dialog_attendance_results.py](src/components/dialog_attendance_results.py) — Dialog to show attendance results and commit logs.
  - [src/components/dialog_share_subject.py](src/components/dialog_share_subject.py) — Share subject code UI.
  - [src/components/dialog_voice_attendance.py](src/components/dialog_voice_attendance.py) — Voice attendance dialog.

- src/database/* — Supabase integration and helper functions
  - [src/database/config.py](src/database/config.py) — Creates the `supabase` client using `st.secrets`.
  - [src/database/db.py](src/database/db.py) — Helper functions: user/teacher creation & login, subject management, attendance logs, password hashing via `bcrypt`.

- src/pipelines/* — AI pipelines
  - [src/pipelines/face_pipeline.py](src/pipelines/face_pipeline.py) — Loads `dlib` models, extracts face embeddings, trains/predicts with `sklearn.SVC`.
  - [src/pipelines/voice_pipeline.py](src/pipelines/voice_pipeline.py) — Uses `Resemblyzer` + `librosa` to extract voice embeddings and identify speakers.

- src/screens/* — Streamlit screens
  - [src/screens/home_screen.py](src/screens/home_screen.py) — Landing page that routes to student or teacher portals.
  - [src/screens/teacher_screen.py](src/screens/teacher_screen.py) — Teacher dashboard: create subjects, take attendance, view records.
  - [src/screens/student_screen.py](src/screens/student_screen.py) — Student flows: face login, registration, enroll/unenroll, view attendance.

- src/ui/base_layout.py — Shared CSS and layout helpers for Streamlit styling.

## Notes and recommendations

- Secrets: Keep `.streamlit/secrets.toml` private and never commit keys.
- Supabase schema: The code expects tables like `teachers`, `students`, `subjects`, `subject_students`, and `attendance_logs`.
- Models: Face pipeline depends on `dlib` and prebuilt face models; ensure compatible OS-level build tools are available when installing.
- Voice pipeline: `librosa` and `Resemblyzer` may require additional OS dependencies (e.g., `ffmpeg`) — check `requirements.txt`.

## Where to look for changes

- To change UI styles: [src/ui/base_layout.py](src/ui/base_layout.py).
- To modify face/voice models: [src/pipelines/face_pipeline.py](src/pipelines/face_pipeline.py) and [src/pipelines/voice_pipeline.py](src/pipelines/voice_pipeline.py).
- To change DB logic: [src/database/db.py](src/database/db.py).

---

If you want, I can:
- Add docstrings to functions in `src/database/db.py` and `src/pipelines/*`.
- Generate a `docs/` folder with per-file detailed docs.
- Run a quick static analysis for missing imports or obvious issues.

Tell me which follow-up you'd like.