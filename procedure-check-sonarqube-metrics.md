Usually, checks are done in Flow in Exit Gates.

However, you may want to force a failure in the middle of other Tasks.

Here is an explanation how to do it.

This page explains how to write a Flow procedure which wil check if the result of the SonarQube Metrics are 'OK'.

### Retrieve the SonarQube Metrics via the Sonar plugin

Use the GetLastSonarMetrics procedure from the SonarQube plugin.

This procedure will collect the metrics and store them (by default) in a property `/myJob/getLastSonarMetrics`.

### Write a procedure to check the value of the Alert_status property

We are interested in the global status of the SonarQube Metrics, which is stored in the `/myJob/getLastSonarMetrics/alert_status` property.

Let's create a procedure with a step whose execution depends on the result of this variable:

`Run condition`: `$[/javascript (getProperty('/myJob/getLastSonarMetrics/alert_status')!='OK');]`


`Command(s)`:
```
echo "Sonar Metrics are not 'OK'. So this task will fail";
ectool setProperty '/myJob/outcome' "error"
ectool setProperty '/myJob/summary' "Sonar Metrics don't reach the expected status. Expected 'OK', got '$[/myJob/getLastSonarMetrics/alert_status]'"
```
