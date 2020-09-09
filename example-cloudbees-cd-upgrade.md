# Backup

Reference: https://docs.cloudbees.com/docs/cloudbees-cd/latest/maintenance/backups/commander-server-backups

## Create backup folder
```
mkdir /opt/electriccloud/backup-2020-09-09 && cd /opt/electriccloud/backup-2020-09-09
```

## Backup database via _ectool export_
```
mkdir database && cd database
ectool login admin <password>
ectool export db.bkp
```

_< wait for the database export >_

```
mv /opt/electriccloud/electriccommander/db.bkp.gz /opt/electriccloud/backup-2020-09-09/database/
```

## Backup _passkey_
```
mkdir /opt/electriccloud/backup-2020-09-09/passkey
cp /opt/electriccloud/electriccommander/conf/passkey /opt/electriccloud/backup-2020-09-09/passkey
```

## Backup _plugins_
```
mkdir /opt/electriccloud/backup-2020-09-09/plugins
cp -R /opt/electriccloud/electriccommander/plugins/* /opt/electriccloud/backup-2020-09-09/plugins
```

## Backup _CloudBees CD configuration_
```
mkdir /opt/electriccloud/backup-2020-09-09/conf
cp -R /opt/electriccloud/electriccommander/conf/* /opt/electriccloud/backup-2020-09-09/conf
```

## Backup _Apache configuration_
```
mkdir /opt/electriccloud/backup-2020-09-09/apache-conf
cp -R /opt/electriccloud/electriccommander/apache/conf/* /opt/electriccloud/backup-2020-09-09/apache-conf
```

# Upgrade the various servers (CD server, Insight server, agents)

## Download and install CD server new version
```
cd
wget https://downloads.cloudbees.com/cloudbees-cd/Release_10.0/10.0.1.143076/linux/CloudBeesFlow-x64-10.0.1.143076
chmod +x CloudBeesFlow-x64-10.0.1.143076
sudo ./CloudBeesFlow-x64-10.0.1.143076
```

```
Upgrade the existing 9.2.0.139827 installation to version 10.0.1.143076? [n/Y] Y
```

_< wait for installation >_

_< wait for server restart and the upgrades being done during startup >_

## Download and install DevOps Insight new version
```
cd
wget https://downloads.cloudbees.com/cloudbees-cd/Release_10.0/10.0.1.143076/linux/CloudBeesFlowDevOpsInsightServer-x64-10.0.1.143076
chmod +x CloudBeesFlowDevOpsInsightServer-x64-10.0.1.143076
sudo ./CloudBeesFlowDevOpsInsightServer-x64-10.0.1.143076
```

```
Version 9.2.0.139827 of CloudBees CD DevOps Insight Server is already installed on this machine.
Upgrade the server to 10.0.1.143076? [n/Y]
```

## Download and install Agent new version
```
cd
wget https://downloads.cloudbees.com/cloudbees-cd/Release_10.0/10.0.1.143076/linux/CloudBeesFlowAgent-x64-10.0.1.143076
chmod +x CloudBeesFlowAgent-x64-10.0.1.143076
sudo ./CloudBeesFlowAgent-x64-10.0.1.143076
```

```
Upgrade the existing 9.2.0.139827 installation to version 10.0.1.143076? [n/Y]
```
