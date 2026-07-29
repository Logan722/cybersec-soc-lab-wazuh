# Configuration

## Wazuh stack

The Wazuh Manager, Indexer, and Dashboard are deployed from the official
[wazuh-docker](https://github.com/wazuh/wazuh-docker) single-node compose, pinned to `v4.14.5`.
No changes were made to the base compose file for Phase 1. The deployment steps are in the
root [README](../README.md).

## Coming in later phases

- `sysmonconfig.xml` — Olaf Hartong sysmon-modular config for the Windows endpoint (Phase 2)
- Agent `ossec.conf` snippets — Sysmon event channel and log source configuration (Phase 2)
