# NoteAgent

NoteAgent is a simple web app built with FastAPI + LangGraph/LangChain.
It runs an AI note assistant that can read and write text files from natural language prompts.

## Features
- FastAPI backend
- LangGraph ReAct-style agent
- Two file tools:
  - `read_note(filepath)`
  - `write_note(filepath, content)`
- Web UI (`templates/index.html`) to send prompts and display responses

## Project Structure

```text
DeployAIAgent/
|- agent.py               # Agent tools, LLM setup, and run_agent()
|- main.py                # FastAPI app and API routes
|- templates/
|  |- index.html          # Frontend page
|- public/
|  |- favicon.ico
|- requirements.txt       # Runtime dependencies
|- pyproject.toml         # Basic Python project metadata
|- .env                   # Environment variables (local only)
|- note.txt               # Sample note file
|- README.md
```

## How It Works
1. User opens `GET /` and sees the HTML interface.
2. User submits a prompt.
3. Frontend sends `POST /agent` with:
   ```json
   { "prompt": "..." }
   ```
4. Backend calls `run_agent()` from `agent.py`.
5. Agent decides whether to use `read_note` or `write_note`.
6. Final response is returned to the UI.

## Requirements
- Python 3.10+
- OpenAI API key

## Local Setup

```bash
# 1) Create virtual environment
python -m venv .venv

# 2) Activate environment
# Windows PowerShell
.venv\Scripts\Activate.ps1

# 3) Install dependencies
pip install -r requirements.txt

# 4) Create .env file
# Add:
OPENAI_API_KEY=your_openai_api_key

# 5) Run server
uvicorn main:app --reload
```

Open:
- `http://127.0.0.1:8000`

## API Endpoints

### `GET /`
Returns the main HTML page.

### `POST /agent`
Invokes the AI agent.

Request body:
```json
{
  "prompt": "Read note.txt"
}
```

Success response:
```json
{
  "response": "..."
}
```

Common errors:
- `400` when `prompt` is empty
- `500` on internal server error

## Example Prompts
- `Read note.txt`
- `Write in note.txt: Meeting at 6 PM`
- `Read C:/Users/BR/Desktop/DeployAIAgent/note.txt`
- `Write in note.txt: TODO - finish report`

## Security Notes
- Do not commit `.env` to GitHub.
- Rotate your API key immediately if it was exposed.
- Current tools can access file paths directly; for production, restrict file access to a safe directory.

## Deployment Notes
You can deploy this as a Python/FastAPI app (for example on Vercel or any ASGI-compatible platform).

Make sure:
- Dependencies are installed from `requirements.txt`
- App entrypoint is `main:app`
- `OPENAI_API_KEY` is set in deployment environment variables

## Current Limitations
- No authentication
- No role-based file permission control
- No test suite yet
- `pyproject.toml` does not include all runtime dependencies currently listed in `requirements.txt`

## Suggested Improvements
- Add auth before allowing file operations
- Restrict tool access to a dedicated notes directory
- Add unit and API tests
- Add structured logging and monitoring
- Add streaming responses for better UX

## License
Add your preferred license (for example MIT) before public release.
