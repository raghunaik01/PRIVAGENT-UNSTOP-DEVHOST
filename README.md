# PRIVAGENT

**Protect Personal Data. Secure Every Tool Call.**

PRIVAGENT is a prototype privacy-and-safety middleware for AI-agent tool calls. It sits between an AI agent and an external business tool, inspects the request context, detects selected PII, evaluates context-fusion risk, and returns an `ALLOW`, `REVIEW`, or `BLOCK` decision before the tool is called.

> Hackathon prototype — not a production security boundary.

## Demo flow

`User Task → AI Agent → PRIVAGENT Middleware → Context Mapping → Context Relationship → Fusion Test → Risk Decision → Tool Interceptor → Business Tool`

### Example

A hotel-booking request may contain travel context and employee/HR context. PRIVAGENT keeps those contexts separate, tests the combined action, and prevents unnecessary sensitive information from reaching the external booking tool.

## Features

- Context-aware request inspection
- Email, phone and Aadhaar detection/tokenization for demo data
- Reversible token vault for the demo session
- Context A / Context B / A+B fusion analysis
- `ALLOW`, `REVIEW`, `BLOCK` decisions
- Tool interception before simulated external-tool execution
- Audit log with decision evidence
- Simple browser-based demo dashboard
- Automated tests

## Project structure

```text
privagent/
├── app/
│   ├── __init__.py
│   ├── main.py
│   ├── models.py
│   ├── pii.py
│   ├── fusion.py
│   ├── middleware.py
│   └── audit.py
├── static/
│   └── index.html
├── tests/
│   └── test_privagent.py
├── .gitignore
├── Dockerfile
├── requirements.txt
└── README.md
```

## Run locally

### 1. Create environment

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scripts\activate
```

macOS/Linux:

```bash
source .venv/bin/activate
```

### 2. Install

```bash
pip install -r requirements.txt
```

### 3. Start

```bash
uvicorn app.main:app --reload
```

Open `http://127.0.0.1:8000`.

## API

### Health

`GET /api/health`

### Inspect a tool request

`POST /api/check`

Example body:

```json
{
  "user_task": "Book a hotel in Delhi from 12-15 Nov",
  "tool": "hotel_booking",
  "payload": {
    "destination": "Delhi",
    "dates": "12-15 Nov",
    "employee_id": "EMP1234",
    "employee_benefits": "Executive travel entitlement"
  },
  "contexts": {
    "A": {
      "name": "Travel",
      "fields": ["destination", "dates"]
    },
    "B": {
      "name": "Employee/HR",
      "fields": ["employee_id", "employee_benefits"]
    }
  }
}
```

The response includes:

- detected PII
- context mapping
- fusion analysis
- risk level
- decision
- protected payload
- audit evidence

## Tests

```bash
pytest -q
```

## Docker

```bash
docker build -t privagent .
docker run -p 8000:8000 privagent
```

## GitHub upload

```bash
git init
git add .
git commit -m "Initial PRIVAGENT prototype"
git branch -M main
git remote add origin https://github.com/<YOUR-USERNAME>/privagent.git
git push -u origin main
```

Replace `<YOUR-USERNAME>` with your GitHub username.
