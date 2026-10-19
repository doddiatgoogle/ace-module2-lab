# AI Agent Guidelines for OWASP Juice Shop

This document is the **primary authoritative source** of context for all AI assistants (Claude, GitHub Copilot, Codeium, Continue.dev, Junie, etc.) contributing to OWASP Juice Shop. It provides comprehensive guidelines to maintain code quality, security, and adherence to project standards.

## Project Overview

- **Project**: OWASP Juice Shop - an intentionally insecure web application for security training
- **Primary Languages**: TypeScript, JavaScript, Angular (frontend)
- **Key Technologies**: Node.js (22–25 with 24 being the default), Express, SQLite/Sequelize, MongoDB/MarsDB, Angular 21.x
- **Testing**: Node.js built-in test runner (server unit tests), Supertest (API integration), Vitest (frontend unit tests), Cypress (E2E tests)
- **Code Style**: JS Standard Style (enforced via ESLint)
- **Repository**: [juice-shop/juice-shop](https://github.com/juice-shop/juice-shop)

## Key Files and Directories

- `app.ts` / `server.ts` - Application entry points
- `lib/` - Utility functions and libraries (including `lib/startup/` for initialization)
- `routes/` - Express route handlers
- `models/` - Sequelize data models (SQLite)
- `data/` - Data creation and management (`data/static/` for challenges, users, codefixes)
- `views/` - Server-rendered templates (Handlebars `.hbs` and Pug `.pug`)
- `test/server/` - Server unit tests (Node.js built-in test runner)
- `test/api/` - API integration tests (Node.js built-in test runner + Supertest)
- `frontend/src/` - Angular frontend code (tests use Vitest)
- `cypress/` - E2E tests (Cypress)
- `rsn/` - Refactoring Safety Net scripts and cache
- `config/` - Configuration files (YAML, multiple themed configs like `ctf.yml`, `default.yml`)
- `i18n/` - Internationalization files (do NOT modify directly)
- `ftp/` - Files served via the simulated FTP directory
- `monitoring/` - Grafana dashboard config
- `.github/workflows/` - CI/CD pipelines
- `encryptionkeys/` - Encryption key files

## Important Constraints

1. **Security Context**: This project contains intentional vulnerabilities for training. New vulnerabilities must be approved by maintainers and well-documented.
2. **Challenge Development**: Consult maintainers before creating new challenges. AI-generated challenges risk being duplicate, unsolvable, or dysfunctional.
3. **Code Changes and RSN**: When modifying challenge-related code, the Refactoring Safety Net must pass.
4. **Dependency Updates**: Verify compatibility with `package.json` and `frontend/package.json`.
5. **Translation Modifications**: Use [Crowdin](https://crowdin.com/project/owasp-juice-shop), not direct file editing.

## Recommended Use Cases

### ✅ Good Use Cases

- **Code Analysis**: Understanding existing code structure and patterns
- **Refactoring**: Improving code quality while maintaining functionality
- **Test Writing**: Creating unit, integration, and e2e tests
- **Bug Fixing**: Identifying and resolving issues
- **Documentation**: Writing clear comments and documentation

### ⚠️ Use with Caution

- **Challenge Development**: Consult with maintainers before creating new challenges.
- **Security Vulnerabilities**: Ensure AI-suggested vulnerabilities are intentional and appropriate for the project.
- **Dependencies**: Verify any suggested package updates for compatibility.
- **Architecture Changes**: Discuss major structural changes with maintainers first.

## Essential Guidelines

### 1. Clean Up AI-Generated Noise

**Required** per CONTRIBUTING.md rule #6: Remove unnecessary AI-generated content before submitting PRs.

**Remove**:
- Verbose comments explaining obvious code
- Generic placeholder comments
- Overly detailed docstrings for simple functions
- Repetitive explanations, `console.log` statements

**Keep**:
- Meaningful comments for complex logic
- Challenge hints and metadata
- Security-relevant documentation

### 2. Code Style Compliance

Always run ESLint before committing (unless only `REFERENCES.md` or `SOLUTIONS.md` were modified):
```bash
npm run lint
```
The AI should suggest code following [JS Standard Style](http://standardjs.com/), but always verify.

### 3. Testing Requirements

For any code changes (unless only `REFERENCES.md` or `SOLUTIONS.md` were modified):
- **Unit/Integration Tests**: New features and changes should have tests.
- **E2E Tests**: Required for new/modified challenges.
- **RSN (Refactoring Safety Net)**: Required when modifying existing code that is part of a coding challenge (see the [verify-rsn-fix skill](./.ai/skills/verify-rsn-fix/SKILL.md) for details).
- **Run Tests Locally**:
  ```bash
  npm test                    # Runs frontend, server, and api tests
  npm run test:frontend       # Frontend unit tests (Vitest)
  npm run test:server         # Server unit tests only (Node.js built-in test runner)
  npm run test:api            # API integration tests (Node.js built-in test runner + Supertest)
  npm start & npm run test:e2e  # E2E tests (Cypress)
  npm run rsn                 # Refactoring Safety Net
  ```

### 4. Commit Sign-off

All commits must be signed off (DCO):
```bash
git commit -s -m "Your commit message"
```

### 5. Branch and PR Strategy

- Work on `develop` branch-based feature branches.
- Keep PRs focused on a single scope.
- Reference related issues in PR descriptions.

## Development Workflow

### 1. Understanding the Codebase
Ask the AI to:
- Explain specific components or patterns.
- Identify where to implement new features.
- Trace code execution paths.

### 2. Implementation
Ask the AI to:
- Generate initial implementation.
- Suggest test cases.
- Review for security implications.

### 3. Quality Assurance
Before committing:
1. Remove AI-generated noise.
2. Run `npm run lint` (unless only `REFERENCES.md` or `SOLUTIONS.md` were modified).
3. Run relevant test suites.
4. If you modified code that is part of a coding challenge, run `npm run rsn`.
5. Manually verify functionality.
6. Check for unintended changes.

### 4. Documentation
Ask the AI to:
- Write clear commit messages.
- Draft PR descriptions.
- Document complex logic.

## Anti-Patterns to Avoid

❌ **Don't**: Accept AI suggestions blindly without understanding them.
✅ **Do**: Review and understand all AI-generated code.

❌ **Don't**: Submit PRs with verbose AI-generated comments.
✅ **Do**: Clean up and keep only meaningful comments.

❌ **Don't**: Skip testing because AI "seems confident".
✅ **Do**: Always run the full test suite.

❌ **Don't**: Use AI for contribution farming or trivial changes.
✅ **Do**: Make meaningful contributions that add value.

❌ **Don't**: Let AI modify translations directly.
✅ **Do**: Use [Crowdin](https://crowdin.com/project/owasp-juice-shop) for translations.

## Example: Implementing a Bug Fix

1. **Analyze**: Ask the AI to analyze the issue.
2. **Locate**: Locate the problematic code.
3. **Implement**: Implement the fix with the AI's help.
4. **Test**: Generate tests and run the suite.
5. **RSN**: Run `npm run rsn` if the fix affects code used in a coding challenge.
6. **Sign-off**: Clean up and commit with sign-off (`git commit -s`).

## Quality Checklist

Before submitting a PR:

- [ ] Code follows JS Standard Style (ESLint passes)
- [ ] AI-generated noise removed
- [ ] Tests added/updated and passing
- [ ] RSN check passing (if modified code relevant for a coding challenge)
- [ ] Manual testing completed
- [ ] Commits are signed off
- [ ] PR based on `develop` branch
- [ ] Single, focused scope
- [ ] All CI checks passing

## Refactoring Safety Net (RSN)

When modifying existing code that is part of a coding challenge, you must run the RSN to ensure code snippet and fix option files remain consistent:

```bash
npm run rsn
```

- **If RSN fails**: Review the listed differences.
- If changes are intentionally part of the coding challenge, update the differences cache: `npm run rsn:update`.
- **IMPORTANT**: Utilize the [verify-rsn-fix skill](./.ai/skills/verify-rsn-fix/SKILL.md).
- When refactoring source code that is part of a challenge snippet, manually apply the same changes to the corresponding codefix files in `data/static/codefixes/` to maintain consistency.

## Getting Help

- **Authoritative Guide**: [AGENTS.md](./AGENTS.md)
- **Contribution Guidelines**: [CONTRIBUTING.md](./CONTRIBUTING.md)
- **Project Documentation**: [pwning.owasp-juice.shop](https://pwning.owasp-juice.shop/)
- **Community**: GitHub issues and discussions.

## Skills

- [add-reference skill](./.ai/skills/add-reference/SKILL.md): Instructions for adding new blog posts, talks, or other references to `REFERENCES.md`
- [add-solution skill](./.ai/skills/add-solution/SKILL.md): Instructions for adding new hacking guides, videos, or tools to `SOLUTIONS.md`
- [create-m3-theme skill](./.ai/skills/create-m3-theme/SKILL.md): Instructions for creating new Angular Material M3 themes
- [generate-release-notes skill](./.ai/skills/generate-release-notes/SKILL.md): Instructions for generating release notes.
- [verify-challenge skill](./.ai/skills/verify-challenge/SKILL.md): Instructions for verifying new challenges fulfill all project requirements and metadata
- [verify-rsn-fix skill](./.ai/skills/verify-rsn-fix/SKILL.md): Instructions for identifying and fixing broken RSN caused by code changes

## Verification of Agent Context

To verify that an AI agent (like GitHub Copilot or Claude) is correctly using this context, you can use the following test prompts:

1. **Check Primary Guidelines**: "What are the security constraints for developing new challenges in this project? Refer to the primary agent guidelines."
   - *Expected Result*: The agent should summarize constraints from the "Important Constraints" section of this file.
2. **Check Skill Discovery**: "How do I fix a break in the Refactoring Safety Net (RSN)? Is there a skill for this?"
   - *Expected Result*: The agent should point to the `verify-rsn-fix` skill located in `./.ai/skills/verify-rsn-fix/SKILL.md`.
3. **Check Skill Content**: "Show me the checklist for verifying a new challenge."
   - *Expected Result*: The agent should find and display the content from `./.ai/skills/verify-challenge/checklists/challenge-checklist.md`.

## Remember

AI agents are productivity tools for enhancing development. You (or the person reviewing the PR) are responsible for the quality, correctness, and security of all contributions. Always review AI-generated code critically, test thoroughly, and follow the project's guidelines.

---

**Last Updated**: April 2026

<!-- do-harness-lite-workspace -->
# AGENTS.md — Rapid ADK & Cloud Engineering Harness (`do-harness-lite`)

## 0. Workspace Specification Precedence (Always Inspect First)
1. **Zero-Config Auto-Discovery (`hl-env` -> `.harness-context.md`)**: Before editing any file, run `hl-env` (or inspect `ls -la`, `spec.md`, `PRD.md`, `README.md`, `GEMINI.md`, `.agents/CONTEXT.md`, `triage/briefs/*.md`, `pyproject.toml`, `agents-cli-manifest.yaml`) and read `.harness-context.md` to lock in the workspace's entrypoints, agents, tools, eval datasets, and live GCP resources.
2. **Preserve Contracts**: Never rename required entry-point files (`app/agent.py`, `main.py`, `<pkg>/__init__.py`), exported symbols (`root_agent`, `app`), function names, parameter names, or port numbers (`8000`, `8080`, `3002`, `5000`) expected by local test or verification scripts.
3. **Preserve Existing Guardrails**: If the workspace already contains `GEMINI.md`, `.agents/CONTEXT.md`, `.agents/hooks.json`, or `.pre-commit-config.yaml`, keep them intact and additive.

## 1. Google ADK & Vertex AI Runtime Invariants (`RULES-AI` Exam-Speed Edition)
- **Model Selection & Lab Provisioned Throughput Precedence**:
  - **Lab Grader & Provisioned Throughput Win First**: In Qwiklabs / Challenge Labs, **always preserve the exact model specified in `spec.md`, `.env`, or starter code** (e.g., `gemini-3.6-flash` on `global`, which has Provisioned Throughput enabled to prevent `429 Resource Exhausted`). NEVER change the model name unless explicitly instructed.
  - **When Unspecified**: Default to `gemini-3.8-flash` (fast/sub-agent) or `gemini-4-argon` (complex orchestrator) on `GOOGLE_CLOUD_LOCATION="global"`.
- **Anti-"Decorative ADK" & Canonical `agents-cli` Contract (`app/agent.py`)**:
  - **No Raw `generate_content` or Hollow `root_agent`**: Never call `client.models.generate_content()` directly inside an agent module (`app/agent.py` or `<pkg>/__init__.py`), and never export a hollow `root_agent = Agent(...)` with `tools=[]` and `sub_agents=[]`.
  - **Export BOTH `root_agent` and `app`**: In `app/agent.py` (and `<pkg>/__init__.py`), always export `root_agent = Agent(...)` AND `from google.adk.apps import App; app = App(root_agent=root_agent, name="app")` so `agents-cli deploy` (`FROM python:3.12-slim` container build) and `agents-cli playground` work on the first try without Python 3.14 `cloudpickle` (`Code 13`) failures.
  - **Thinking Token Budget & Tool Safety**: When setting `generate_content_config`, use `types.GenerateContentConfig(max_output_tokens=32768)` so thinking tokens never truncate output (`MAX_TOKENS`). Never pass user/LLM input to `eval()`, `exec()`, `subprocess.run(..., shell=True)`, or unparameterized SQL strings.
- **Global vs. Regional Endpoint Split**:
  - **Gemini Model Inference**: When using **`Gemini 4 Argon` (`gemini-4-argon`)**, **`Gemini 3.8 Flash` (`gemini-3.8-flash`)**, or legacy lab models (`gemini-3.6-flash`, `gemini-3.1-pro-preview`, `gemini-3-flash-preview`) on Vertex AI, set `GOOGLE_GENAI_USE_VERTEXAI="True"` and `GOOGLE_CLOUD_LOCATION="global"` in `.env` (or `os.environ` before initializing `genai.Client()` / `Agent()`).
  - **Agent Runtime (`ReasoningEngine`), Vertex AI `EvalTask`, & Cloud Run**: Always use the **regional** location (`us-central1`, `europe-west1`, etc.) in `agents-cli-manifest.yaml`, `--region`, and `vertexai.init(project=..., location=...)`.
  - **Model Armor**: Always use the regional REP endpoint `modelarmor.<region>.rep.googleapis.com` (`ClientOptions(api_endpoint=f"modelarmor.{location}.rep.googleapis.com")`).
- **Agent Identifier Naming**: Every `Agent(name=...)`, `LlmAgent`, `SequentialAgent`, `ParallelAgent`, and `LoopAgent` MUST use a valid Python identifier with **underscores, never hyphens** (`^[a-zA-Z_][a-zA-Z0-9_]*$`).
- **Python Function Tools (`tools=[...]`)**:
  - Every tool function MUST have explicit parameter type annotations, a return type annotation, and a docstring describing *when* and *why* to call it (plus an `Args:` block).
  - Never use non-existent decorators like `@root_agent.skill` or `@agent.tool`; pass plain typed functions directly into `tools=[my_func]`.
  - Define all tool functions and callbacks **above** the `Agent(...)` constructor that references them.
- **Multi-Agent Routing & Single-Parent Rule**:
  - Every specialist agent in `sub_agents=[...]` or `AgentTool(agent=...)` MUST have a clear, non-overlapping `description`.
  - In ADK, an `Agent` instance can belong to **at most one `parent_agent`**. Never pass the same sub-agent instance to two parents; instantiate fresh instances via a factory function if needed.
  - When mixing `VertexAiSearchTool` with custom Python function tools on a root agent, wrap the `VertexAiSearchTool` agent inside `AgentTool(agent=search_agent)` in `tools=[...]`.
- **Session State Placeholders & Scoping**:
  - Always append a trailing `?` to state placeholders in `instruction=` strings (e.g., `{intake_summary?}`) so Turn 1 never fails with `KeyError` before a tool or upstream agent populates the key.
  - In `SequentialAgent`, ensure Step 1 sets `output_key="my_key"` when Step 2 references `{my_key?}`.
  - Use the 4 ADK state scopes deliberately: unprefixed `state["k"]` (current session), `state["user:k"]` (cross-session per user), `state["app:k"]` (global app), `state["temp:k"]` (current turn only).
- **ADK Callback Return Semantics**:
  - `before_tool_callback(tool: BaseTool, args: dict, tool_context: ToolContext) -> dict | None`: Return `None` to **allow** tool execution; return a `dict` (e.g., `{"status": "error", "reason": "Blocked by policy"}`) to **block/short-circuit** the tool call.
  - `before_model_callback(callback_context: CallbackContext, llm_request: LlmRequest) -> LlmResponse | None`: Mutate `llm_request.contents` in place and return `None` to proceed to the model, or return an `LlmResponse` to short-circuit inference.
- **Deployment Target Separation (`Agent Runtime` vs. `Cloud Run` — NEVER MIX)**:
  - **Target A — Vertex AI Agent Runtime (`ReasoningEngine` / `deployment_target: "agent_runtime"`)**:
    - **NEVER set `PORT` in `.env`, `Dockerfile`, or `env_vars`**: `PORT` is a reserved environment variable on Agent Runtime / Cloud Run (`400 FAILED_PRECONDITION: Environment variable name 'PORT' is reserved`). Remove `PORT` from `.env` and `Dockerfile` before running `agents-cli deploy`.
    - **NEVER use `__file__` inside Tool Functions (`Code 13` Pickle Trap)**: When `ReasoningEngine.create` / `client.agent_engines.create` serializes a tool function with `cloudpickle`, `__file__` either raises `NameError` in the remote container or hardcodes the local VM path (`/config/Desktop/...`), causing `Code 13: Reasoning Engine failed to deploy`. Inside every tool function, import `os` locally inside the function body, check relative/container paths (`"corporate_travel_policy.txt"`, `"/code/corporate_travel_policy.txt"`, `"/app/corporate_travel_policy.txt"`) inside `try/except`, and embed a fallback policy/data summary string directly inside the function.
    - **Ensure `cloudpickle` is installed**: Run `sudo /opt/venv/bin/pip install -q cloudpickle` (or `pip install -q cloudpickle`) before using `ReasoningEngine.create` / `client.agent_engines.create`.
    - **Avoid Circular Imports Across Subdirectories**: Never put `import agent` inside `app/agent.py` or `<pkg>/agent.py` (when `sys.path[0]` is the subdirectory, `import agent` imports itself!). Keep `root_agent` in a single canonical module (`<pkg>/__init__.py` or `<pkg>/agent.py` imported via `from .agent import root_agent`).
    - Prefer `hl-adk` one-shot subcommands over ad-hoc `python3 -c` scripts: `hl-adk deploy-runtime us-central1`, `hl-adk test-runtime us-central1 "<prompt>"`, `hl-adk publish-ge "<App Name or ID>"`, and `hl-adk run-eval evaluation.json <experiment-name> us-central1`.
  - **Target B — Cloud Run / FastAPI Container (`deployment_target: "cloud_run"` or local `:8000` service)**:
    - When mounting ADK on FastAPI in a container with a read-only root filesystem, call `get_fast_api_app(agents_dir=".", web=False)` (never pass the non-existent `adk_app=` kwarg, and **NEVER** iterate `[r.methods for r in app.routes]` because Starlette `APIWebSocketRoute` has no `.methods` attribute; use `getattr(r, "methods", ["WS"])` if inspecting routes).
    - When updating an existing Cloud Run service's image or environment variable, ALWAYS use `gcloud run services update <svc> --region=<region> --image=<img_uri>` or `--update-env-vars="KEY=VAL"`. NEVER use `--set-env-vars` on an existing service unless explicitly replacing all variables, as it wipes existing database and VPC settings.
- **Vertex AI `EvalTask` & `agents-cli eval run` Invariants**:
  - Always ensure the `default` Vertex AI `MetadataStore` exists in the regional location (`us-central1`) before running `EvalTask`.
  - Use ONLY valid `MetricPromptTemplateExamples.Pointwise` constants (`GROUNDEDNESS`, `QUESTION_ANSWERING_QUALITY`, `INSTRUCTION_FOLLOWING`, `SAFETY`, `"rouge_l_sum"`). **NEVER** reference `.RELEVANCE` (does not exist and raises `AttributeError`).
  - For `agents-cli eval run`, ensure `agents-cli-manifest.yaml` exists at the project root and never pre-create a flat file at the `--output <PATH>` path (`agents-cli` treats `--output` as a directory).

## 2. Anti-Hallucination & Authoritative Introspection (`R5` & `R1`)
1. **Zero-Guessing SDK/CLI Verification**: Never guess unfamiliar `google-adk`, `vertexai.evaluation`, `modelarmor_v1`, or `gcloud` flags/signatures from training data. Query the `developerKnowledge` MCP server (`search_documents` / `get_documents`) when available, or run a 1-second local introspection against the VM's installed packages (`python3 -c "import inspect, ...; print(inspect.signature(...))"` / `gcloud <group> --help`).
2. **Read-Only Data & Schema Discovery First (`R5` / `R15`)**: Inspect live table schemas (`execute_sql_readonly` or `INFORMATION_SCHEMA.COLUMNS`) before writing parameterized SQL tools (`$1` / `:param` in `tools.yaml` or `data_connectors.py`). Never expose raw unparameterized `execute_sql(sql: str)` to an LLM agent.
3. **Sub-Agent Context Buffer (`R1`)**: In multi-ticket Capstone workspaces, delegate broad log/brief scanning (`triage/briefs/*.md`, stack traces) to a read-only sub-agent that returns a compact `file:line` table so the main context stays fast and clean.

## 3. Fast Verification Loop, Test Integrity & 3-Turn Circuit Breaker
1. After every code change, run `hl-verify .` (or `python3 ~/do-harness-lite/scripts/verify_lite.py .`) plus any workspace test/verification command (`pytest`, `npm test`, `hl-adk test-runtime`, or ticket verify script).
2. **Never Weaken Lab Tests (`TEST_SUITE_WEAKENED`)**: Never delete existing test functions or add `@pytest.mark.skip` / `pytest.skip()` to pass `pytest`—fix the underlying implementation so hidden Qwiklabs grading checks pass.
3. Never declare a task complete until `hl-verify .` exits with code `0` and the target deployment (`ReasoningEngine` for Agent Runtime, or `localhost:8000` / Cloud Run URL for FastAPI services) is verified responding.
4. **3-Turn Debug-Spiral Circuit Breaker**: If 3 consecutive attempts fail on the same error, STOP editing code. Run `hl-env`, read the full traceback and logs, state the failed assumption, and verify the exact API/schema before making the next surgical edit.

