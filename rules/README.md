# Detection Rules

Custom Wazuh detection rules live here in `local_rules.xml`, added in Phase 3.

Planned coverage:

- **12 rules mapped to MITRE ATT&CK** — execution, persistence, privilege escalation, defense evasion, credential access, discovery, and command-and-control (Phase 3)
- **6 rules for AI/agent threats** — mapped to MITRE ATLAS and the OWASP Agentic Top 10: shadow AI, LLM data exfiltration, and MCP/agent activity (Phase 4)

Each rule is validated against a real triggered event before it's committed here.
