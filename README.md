# Pentester71

**An autonomous web-application & API security scanner where the LLM _proposes_ and a deterministic gate
_disposes_.** Every finding is confirmed by a non-LLM **oracle with a negative control** — so precision is
*measured*, not claimed.

> 📁 **The project lives in [`Pentester71/`](Pentester71/).**
> Full documentation: **[`Pentester71/README.md`](Pentester71/README.md)**.

---

> ## ⚠️ Authorized use only
>
> Pentester71 sends real, active security probes to whatever target you point it at. **Only use it against
> systems you own or have explicit, written permission to test.** Unauthorized scanning is illegal in most
> jurisdictions and you are solely responsible for how you use this tool. Built-in guardrails (scope allowlist,
> per-engagement authorization reference, non-destructive-by-construction probes, budget ceilings) are aids,
> not a substitute for authorization.

---

## Why it's different

Most AI security tools let a language model *judge* whether something is vulnerable — which produces confident,
plausible, wrong answers. Pentester71 never does that:

- **The LLM proposes; a deterministic gate disposes.** The model may hypothesize, plan, and explain — but it
  **never** confirms a vulnerability, decides scope, or decides when to stop.
- **A finding requires proof-of-control via a passing oracle** with a **negative control** (the same check
  against a secure input must *not* fire).
- **Three states, never two:** `CONFIRMED` / `NOT_VULNERABLE` / `INCONCLUSIVE`. A check that couldn't run is
  never a false "not vulnerable."

On the deliberately-vulnerable ground-truth targets it's validated against (OWASP **VAmPI**, **crAPI**,
**Juice Shop**, self-hosted **GitLab**) it runs at **zero false positives**.

## Safety by design

Scope is a hard allowlist/SSRF gate on every request · no engagement runs without a recorded authorization
reference · a program can declare automated testing *banned* · proof primitives are non-destructive by
construction (mutating checks are write-gated) · bounded cycles / wall-clock / spend · immutable audit trail +
proof-preserving PII redaction.

## Quick start

```bash
git clone https://github.com/secsalem/test.git
cd test/Pentester71

python -m venv venv && source venv/bin/activate     # Windows: venv\Scripts\activate
pip install -r requirements.txt
playwright install chromium

python -c "import secrets; print('API_KEY=' + secrets.token_hex(32))" > .env
uvicorn app.main:app --port 8000
```

Open **http://localhost:8000**, paste your `API_KEY` at the login overlay, enter an **authorized** target, and
scan. See **[`Pentester71/README.md`](Pentester71/README.md)** for configuration, the API, scan profiles, the
full list of confirmed vulnerability classes, and the project layout.

## Stack

Python · FastAPI/uvicorn · httpx · Playwright · SQLAlchemy/SQLite · (optional) Anthropic API.

## Status & disclaimer

Research-grade and evolving; validated against deliberately-vulnerable ground-truth targets. Provided
**as-is, without warranty** — the authors accept no liability for misuse or damage. No license is set yet.
