# Scenario 01 - Critical File Integrity Monitoring

## Objective

Detect in real time the modification of a monitored critical file and generate a high-severity custom Wazuh alert.

## Test Environment

- Endpoint: Ubuntu 24.04 LTS
- Agent: Wazuh Agent 4.14.8
- Monitoring mode: Real-time FIM
- Test file: `/etc/wazuh-pfe-test.conf`

## Detection Process

The Wazuh Agent monitors the `/etc` directory using File Integrity Monitoring (FIM).

When `/etc/wazuh-pfe-test.conf` is modified, Wazuh detects the integrity change and generates the default rule:

- Rule ID: `550`
- Description: `Integrity checksum changed`
- Severity: Level 7

A custom rule was created to specifically identify modifications to this test file and increase the alert severity.

## Custom Rule

```xml
<group name="local,pfe_custom,fim_critical,">

  <rule id="100002" level="12">
    <if_sid>550</if_sid>
    <field name="file">^/etc/wazuh-pfe-test\.conf$</field>
    <description>PFE: Critical configuration file modified: $(file)</description>
    <mitre>
      <id>T1565.001</id>
    </mitre>
  </rule>

</group>
```

## Result

The modification successfully triggered the custom detection:

- Rule ID: `100002`
- Severity: Level 12
- Event type: `modified`
- Monitoring mode: `realtime`
- MITRE ATT&CK: `T1565.001 - Stored Data Manipulation`
- Tactic: `Impact`

Wazuh also recorded the file metadata and hashes before and after the modification, providing evidence that the file content changed.

## SOC Analysis

This scenario demonstrates how a generic FIM event can be enriched with a custom detection rule for a specific high-value file.

In a production environment, the same approach can be applied to sensitive configuration files to prioritize unexpected modifications during SOC investigations.
