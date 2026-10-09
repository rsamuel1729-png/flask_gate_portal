# GATE CSE Prep Portal (Flask + MongoDB)

## Start locally (PowerShell)

MongoDB Server should be running as the Windows `MongoDB` service. The default connection is
`mongodb://127.0.0.1:27017`, database `gate_portal`.

```powershell
Get-Service MongoDB
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe app.py
```

Open http://127.0.0.1:5000. If the service is stopped, open PowerShell as Administrator and run
`Start-Service MongoDB`.

## MongoDB setup and existing data

PyMongo is the Flask application's MongoDB driver. On startup, the app connects to MongoDB and creates indexes.
On the first startup only, if `gate_portal` has no subject data and the old `gate.db` exists, the app copies its
collections into MongoDB while preserving IDs. The SQLite file is left in place as a backup; MongoDB becomes the
database used by the app. Existing users, password hashes, progress, notes, questions and attempts are migrated.

Configuration environment variables:

```powershell
$env:MONGO_URI = "mongodb://127.0.0.1:27017"
$env:MONGO_DATABASE = "gate_portal"
$env:SECRET_KEY = "replace-with-a-long-random-secret"
$env:ADMIN_EMAIL = "admin@gate.local"
$env:ADMIN_PASSWORD = "change-this-password"
```

Set these before starting `app.py`. The MongoDB service is installed locally on the configured machine; the
application does not install the server itself.

## Admin and question imports

Open http://127.0.0.1:5000/admin/login. On a fresh database, the default local login is
`admin@gate.local` / `admin123`. Existing migrated admin accounts keep their original password hashes.

Admins can post exam updates, add syllabus topics and resources, add single MCQs, import CSV question banks, or
upload text-based PDF and DOCX question documents. Document uploads are previewed and editable before confirmation.
Question banks are stored in MongoDB and divided into sets of up to 30; learner mocks serve unseen questions first.
Scanned image PDFs require OCR before upload.

## Other settings

`GATE_EXAM_DATE` uses `YYYY-MM-DD`. `GATE_FEED_URL` can point to an optional RSS/Atom feed. Verify exam dates on
the official GATE site.
