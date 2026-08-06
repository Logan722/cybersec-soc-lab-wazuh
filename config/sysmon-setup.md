# Sysmon Setup (Phase 2)

Sysmon gives the Windows endpoint high-fidelity telemetry — process creation, network
connections, file writes, registry changes, image loads — well beyond the default Windows
logs. The Wazuh agent forwards these events to the manager, and the Phase 3 detection rules
are written against them.

## What was installed

- **Sysmon** (Microsoft Sysinternals) on the Windows endpoint (`win-ep1`)
- **Olaf Hartong `sysmon-modular`** config — the pre-merged balanced / medium-verbosity
  `sysmonconfig.xml` (see `config/sysmonconfig.xml`)

## Install (on the Windows endpoint, PowerShell as Administrator)

Download Sysmon and the config:

```powershell
New-Item -ItemType Directory -Force -Path C:\Sysmon | Out-Null
Invoke-WebRequest -Uri "https://download.sysinternals.com/files/Sysmon.zip" -OutFile "C:\Sysmon\Sysmon.zip" -UseBasicParsing
Expand-Archive -Path "C:\Sysmon\Sysmon.zip" -DestinationPath "C:\Sysmon" -Force
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/olafhartong/sysmon-modular/master/sysmonconfig.xml" -OutFile "C:\Sysmon\sysmonconfig.xml" -UseBasicParsing
```

Install Sysmon with the config:

```powershell
cd C:\Sysmon
.\Sysmon64.exe -accepteula -i sysmonconfig.xml
```

Sysmon now logs to `Microsoft-Windows-Sysmon/Operational` in the Windows Event Log.

## Point the Wazuh agent at the Sysmon channel

By default the Wazuh Windows agent reads the standard logs (Application, Security, System)
but **not** the Sysmon channel. Add this block to `C:\Program Files (x86)\ossec-agent\ossec.conf`,
then restart the agent (`Restart-Service WazuhSvc`):

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

## Verification

Generated benign test activity on the endpoint (a process creation and a registry write),
then confirmed the events arrived in the Wazuh dashboard — including a registry-write event
mapped by Wazuh to **MITRE ATT&CK: Modify Registry**. That's the telemetry the Phase 3 rules
build on.

Note: Wazuh only stores events that match a rule (an alert). A lot of benign Sysmon activity
is level 0 and is not indexed by default — which is precisely why custom detection rules are
the next phase.

## References

- Sysmon — https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
- Olaf Hartong sysmon-modular — https://github.com/olafhartong/sysmon-modular
