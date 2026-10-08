# GATE CSE Prep Portal (Flask)

## Run locally

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
py app.py
```

Open http://127.0.0.1:5000.

## Admin login

Open http://127.0.0.1:5000/admin/login. Default local credentials are `admin@gate.local` / `admin123` on a fresh database.
Set `ADMIN_EMAIL` and `ADMIN_PASSWORD` to configure the initial admin credentials. Change the defaults before deploying.

Admins can post exam announcements, add syllabus topics, and add study resources. Questions can be entered individually,
imported from CSV, or extracted from text-based PDF and DOCX documents.

## Importing question documents

Open the Admin console and choose a subject, optionally a default topic, and a PDF or DOCX file up to 20 MB. The
document must contain numbered MCQs, four labeled options, and an explicit correct answer for each question. The
[question document example](static/question_document_template.txt) shows the format. The app extracts the questions,
shows a preview and warnings, then waits for admin confirmation before importing. Image-only scanned PDFs need OCR
before upload. Extraction can vary with document layout, so review the preview and answer keys before confirming.

Imported questions are shuffled and grouped into ordered sets of up to 30. Thus 90 questions create three sets; 40
create one set of 30 and a second set of 10. Each student's mocks serve unseen questions first, in set order. If fewer
than 30 unseen questions remain, the mock includes those carryover questions and fills the rest with questions that
student has already answered. The portal tracks question usage separately for each student. Mocks remain capped at
30 questions, with up to five tests per day and a 40-minute wait after each submitted test.

PDF/DOCX extraction is not perfect, particularly for scanned pages and complex layouts. The app does not scrape
online questions; only import content you have permission to use.

## Other settings

Environment variables: `SECRET_KEY`, `ADMIN_EMAIL`, `ADMIN_PASSWORD`, `GATE_EXAM_DATE` (YYYY-MM-DD), and
`GATE_FEED_URL` (optional RSS/Atom feed). Verify exam dates against the official GATE website.
