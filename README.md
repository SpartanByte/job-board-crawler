# Job Board Crawler

A small Python automation project for pulling job listings from a source API and importing them into a Notion database. The repository is set up to run the import job both locally and automatically through GitHub Actions.

## Overview

The workflow is designed to:

- fetch recent job postings from an external source
- normalize the data into a consistent format
- send the records to a Notion data source for tracking and review

The automation entry point is configured in the GitHub workflow to run:

```bash
python src/import_jobs.py
```

## Python setup

This project targets Python 3.12, as defined in the GitHub workflow.

### Local environment

```bash
python3.12 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Required environment variables

The import script expects these values to be available at runtime:

- `NOTION_TOKEN`
- `NOTION_DATA_SOURCE_ID`
- `SOURCE_API_KEY`

Example:

```bash
export NOTION_TOKEN="your_notion_token"
export NOTION_DATA_SOURCE_ID="your_notion_database_id"
export SOURCE_API_KEY="your_source_api_key"
python src/import_jobs.py
```

## GitHub workflow

The workflow file is located at `.github/workflows/import-jobs.yml`.

### Triggers

- manual trigger via `workflow_dispatch`
- scheduled daily run at `15:15 UTC` using the cron expression:

```cron
15 13 * * *
```

### Workflow behavior

Each run does the following:

1. checks out the repository
2. sets up Python 3.12
3. installs dependencies from `requirements.txt`
4. runs the import script with the required secret values injected from GitHub Actions secrets

### Required GitHub secrets

Set these in the repository settings under GitHub Actions secrets:

- `NOTION_TOKEN`
- `NOTION_DATA_SOURCE_ID`
- `SOURCE_API_KEY`

The workflow passes them into the job as environment variables before executing the script.

## Expected project layout

```text
.
├── .github/
│   └── workflows/
│       └── import-jobs.yml
├── src/
│   └── import_jobs.py
├── requirements.txt
├── README.md
└── .venv/   # local virtual environment
```

## Notes

This repository is intentionally lightweight and focused on automation. The main operational responsibility is scheduled data ingestion, so most of the project effort should stay centered around the Python import script, dependency management, and the workflow secrets configuration.
