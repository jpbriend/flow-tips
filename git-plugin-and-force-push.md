This page describes how to solve an issue with the `Git` plugin and its `CollectReportingData` procedure if somebody does a `git push --force` on the repository used.

## Symptoms

The job fails with such error:
```
Retrieving metrics using following command:
git log HEAD^..0bec971b283924ffee3775105120057852b8989a  --numstat --date=iso8601 --summary --shortstat
Error: Return (128) from RunCommand. at (eval 32) line 363.
fatal: Invalid revision range HEAD^..0bec971b283924ffee3775105120057852b8989a
```

## Solution

* Go to the old UI: example: https://flow.cloudbees.guru/commander
* Go to **Administration** - **Plugins** - **ECSCM-Git** - **Properties** - **ecreport_data_tracker**
* You should find a property named like `ECSCM-Git-xxxxxxxx-code_commit` with *xxxxxxxx* being the name of the Git repository.

Now update the value or remove the property and the procedure will not fail anymore.
