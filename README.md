# Clara Pipeline

Automated n8n workflows that convert call transcripts into structured artifacts for configuring a Clara voice agent.
## Demo Loom Video
Loom Link: https://www.loom.com/share/e7f560dbb55d4a4ca9be677f331f7c0e
## Pipelines

1. **Pipeline A: Demo -> V1**
- Input: demo call transcript
- Output: V1 `account_memo.json` and `agent_spec.json`

2. **Pipeline B: Onboarding -> V2**
- Input: onboarding transcript + V1 memo/spec
- Output: V2 `account_memo.json`, `agent_spec.json`, and `changelog.json`

## Project Structure

```text
clara-pipeline/
|- data/
|  |- demo_transcript.txt
|  |- onboarding_transcript.txt
|  `- prompts/
|- workflows/
|  |- pipeline_a.json
|  |- pipeline_b.json
|  `- workflow_screenshots/
|- outputs/
|- docker-compose.yml
|- requirements.txt
`- tasks.json
```

## Prerequisites

- Docker + Docker Compose
- Groq API key
- n8n (via Docker service in this repo)

## Run Locally

1. Start n8n:
```bash
docker compose up -d
```

2. Open n8n:
- URL: `http://localhost:5678`
- Username: `admin`
- Password: `admin123`

3. In n8n, create credentials for Groq:
- Type: HTTP Header Auth
- Name: `Groq API`
- Header: `Authorization`
- Value: `Bearer <YOUR_GROQ_API_KEY>`

4. Import workflows:
- `workflows/pipeline_a.json`
- `workflows/pipeline_b.json`

## Pipeline A Usage (Demo -> V1)

1. Open Pipeline A.
2. In **Input Data** node, paste demo transcript (or copy from `data/demo_transcript.txt`).
3. Set `account_id` (example: `bens_electric_solutions`).
4. Run workflow manually.
5. Save final output fields:
- `account_memo_v1` -> `outputs/<account_id>/v1/account_memo.json`
- `agent_spec_v1` -> `outputs/<account_id>/v1/agent_spec.json`

## Pipeline B Usage (Onboarding -> V2)

1. Open Pipeline B.
2. In **Input Data** node, provide:
- onboarding transcript
- `v1AccountMemo`
- `v1AgentSpec`
3. Run workflow manually.
4. Workflow writes:
- `/outputs/<account_id>/v2/account_memo.json`
- `/outputs/<account_id>/v2/agent_spec.json`
- `/outputs/<account_id>/v2/changelog.json`
- `/outputs/tasks.json` (task append)

## Prompt Files

Used from `data/prompts/`:
- `groq_demo_system.txt`
- `groq_demo_user.txt`
- `groq_onboarding_system.txt`
- `groq_onboarding_user.txt`
- `agent_system_prompt_template.txt`


## Outputs in This Repo

Example account artifacts already exist under:
- `outputs/bens_electric_solutions/v1/`
- `outputs/bens_electric_solutions/v2/`

## Tech Stack

- n8n
- Groq chat completions API
- JSON-based pipeline outputs

## License

No license file is currently included.
