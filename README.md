# copytrader
Python copy-trading script to copy Futures positions between Kraken acounts.

## How to use
1. Fork this repository
2. Add the required secrets
3. Enable GitHub Actions
4. Add external scheduling

## Required secrets
- KRAKEN_SOURCE_KEY
- KRAKEN_SOURCE_SECRET
- KRAKEN_YOUR_KEY
- KRAKEN_YOUR_SECRET

Add these in: Settings > Secrets and variables > Actions > New repository secret

## Enable GitHub Actions

Enable Actions at repository level:
1. Click the Settings tab
2. In the left sidebar, click Actions > General
3. Under Actions permissions, select: Allow all actions and reusable workflows. Click Save
4. Under Workflow permissions, select: Read repository contents. Click Save
  
Enable Actions for a fork:
- Click the Actions tab
- Click “I understand my workflows, go ahead and enable them”

Test via "Run workflow"

## Add Scheduling (cron-job.org)

This project does **not** use GitHub’s internal scheduler.  
All scheduled execution is done via **cron-job.org**, which reliably triggers the workflow using `workflow_dispatch`.

Why cron-job.org?
- GitHub’s internal scheduler can pause after repository inactivity (especially in private forks)
- cron-job.org runs indefinitely and predictably
- You can fully control start / stop without touching GitHub config

### Create a GitHub Personal Access Token (PAT)

You need a token that allows cron-job.org to trigger the workflow.

1. GitHub → Settings
2. Developer settings
3. Personal access tokens
4. Fine-grained tokens
5. Generate new token

Configure the token as follows:
- Repository access: Only select this repository (your fork)
- Permissions:
  - Actions → **Read and write**
- Expiration: Choose No expiration if you want it to keep running

Copy the token immediately — you will not see it again.

### Create the cron-job.org trigger

1. Log in to https://cron-job.org
2. Click Create cronjob

Basic settings
- Title:  `GitHub – run workflow`
- URL:  `https://api.github.com/repos/<OWNER>/<REPO>/actions/workflows/run.yml/dispatches`
- Replace:
- `<OWNER>` with your GitHub username
- `<REPO>` with your repository name
- Execute: every 5 minutes
Advanced settings
- Headers:
  - Accept: application/vnd.github+json
  - Authorization: Bearer YOUR_GITHUB_TOKEN
  - Content-Type: application/json
- Request method: POST
- Request body: `{"ref":"main"}`

## To pause / resume
- Pause: Disable the cron-job.org job
- Resume: Re-enable it

No changes in GitHub are required to stop execution.

## To update the script
- Click "Sync fork" in GitHub
