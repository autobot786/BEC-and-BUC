# BEC Individual Email Compromise Playbook (Repo Add-on)

This folder is designed to be copied into an existing incident-response repository.

## What’s included
- `playbook.md` — full IEC playbook
- `notebooks/` — a guided `.ipynb` that helps you run the playbook and generate an incident report skeleton
- `templates/` — markdown templates for interview + report + containment checklist
- `queries/` — example queries for Microsoft 365 and Google Workspace
- `outputs/` — generated artifacts (kept in repo by default; adjust to your policy)

## How to add to your GitHub repo
1. Copy the entire folder into your repo (e.g., `playbooks/bec-individual-email-compromise-playbook/`).
2. Add to your top-level `README` or IR index.
3. (Optional) Add `outputs/` to `.gitignore` if you don’t want generated incident files committed.

## Running the notebook
Open `notebooks/BEC_Individual_Email_Compromise_Playbook.ipynb` in JupyterLab or VS Code.
Edit the incident context cell, run cells top-to-bottom, and export the generated markdown report from `outputs/`.
