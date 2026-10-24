# Scenario 03 - Privileged Command Monitoring

## Objective

Monitor privileged activity performed with `sudo` and create a custom Wazuh rule for a command selected as sensitive within the laboratory.

## Test Environment

- Endpoint: Ubuntu 24.04 LTS
- Log source: systemd / journald
- Wazuh Agent: 4.14.8
- Monitored command: `/usr/bin/whoami`

## Initial Detection

The following command was executed:

```bash
sudo whoami
```

The command executed successfully as `root`.

Wazuh detected the sudo event using its native Rule `5402`:

- Rule ID: `5402`
- Description: `Successful sudo to ROOT executed.`
- Severity: Level 3
- Source user: `assia_eddaif`
- Destination user: `root`
- Command: `/usr/bin/whoami`
- MITRE ATT&CK: `T1548.003 - Sudo and Sudo Caching`

## Custom Detection Rule

The native Rule `5402` detects successful sudo activity with a severity level of 3.

For this laboratory, `/usr/bin/whoami` was intentionally selected as a monitored command to demonstrate command-specific detection and severity escalation.

The following custom rule was created:

```xml
<group name="local,pfe_custom,sudo_sensitive,">

  <rule id="100003" level="10">
    <if_sid>5402</if_sid>
    <field name="command">^/usr/bin/whoami$</field>
    <description>PFE: Sensitive command executed with sudo: $(command)</description>
    <mitre>
      <id>T1548.003</id>
    </mitre>
  </rule>

</group>
```
## Result

Executing `sudo whoami` successfully triggered the custom detection:

- Rule ID: `100003`
- Severity: Level 10
- Command: `/usr/bin/whoami`
- Source user: `assia_eddaif`
- Destination user: `root`
- MITRE ATT&CK: `T1548.003 - Sudo and Sudo Caching`

## SOC Analysis

This scenario demonstrates how native Wazuh detections can be extended with custom rules to monitor specific privileged activities.

The `whoami` command is not inherently malicious. It was selected as a harmless laboratory trigger to demonstrate command-based detection without executing a destructive operation.

In a production environment, similar rules could be adapted to monitor commands or administrative actions considered sensitive by the organization.
