# Validation & Testing

Evidence that the detections actually fire.

## Phase 1 — pipeline validation (done)

The end-to-end pipeline was validated before any custom rules were written: an SSH
brute-force test against `linux-ep1` (repeated failed logins to a non-existent user)
produced **18 authentication-failure alerts** in the dashboard, automatically mapped by
Wazuh's built-in ruleset to **MITRE ATT&CK T1110 (Brute Force)** and **T1110.001
(Password Guessing)**. This confirmed the full path works: endpoint → agent → manager →
indexer → dashboard.

## Coming in later phases

- `atomic-tests-used.md` — the Atomic Red Team test mapped to each custom rule (Phase 5)
- `docs/screenshots/` — a screenshot of every rule firing on its matching attack (Phase 5)
- `ai-triage/` — the local LLM triage script, its real output, and an honest evaluation of where the model helped and where it misled (Phase 6)
