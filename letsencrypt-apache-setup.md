This page describes how to setup a Let's Encrypt certificate for a Flow server (Apache SSL configuration only) and have automatic renewal of the certificate.

It does not impact the certificates loaded in the Flow engine. Only the web server certificates will be impacted.

Following content has been written for Ubuntu. It should be easy to adapt for any other OS (check Certbot installation).

## Prerequisites
* Port `80` opened on Flow server
* have `sudo` access
* Flow server must have `access to Internet`

## Backup
As always, backup these 2 files in case something goes wrong:
* `/opt/electriccloud/electriccommander/apache/conf/server.crt`
* `/opt/electriccloud/electriccommander/apache/conf/server.key`

## Install Certbot
```
sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

## Setup a domain name
Use your DNS to setup a domain name pointing to your Flow server.

## Get the certificate for the 1st time
Beware: this step will stop the Flow web server. Thus Flow UI will not be accessible during these steps.

* Stop the Flow web server

`sudo systemctl stop commanderApache.service`

* Generate the certificate using Certbot. Enter your domain name when asked.

`sudo certbot certonly --standalone`

* Restart the Flow web server

`sudo systemctl start commanderApache.service`

The certificate and the private keys have been generated here (by default) (_domain.name_ is the domain you provided earlier):
* Certificate is generated here : `/etc/letsencrypt/live/domain.name/fullchain.pem`
* Private key is generated here: `/etc/letsencrypt/live/domain.name/privkey.pem`

These files will be copied to the Apache server during the last step, when we will do a dry-run of the automatic renewal.

# Apache configuration
The default Flow Apache configuration is located in this directory: `/opt/electriccloud/electriccommander/apache/conf`
The `ssl.conf` file contains everything related to SSL configuration.
Interesting parameters in this file are:
* `ServerName` (the value of your domain name)
* `SSLCertificateFile` (default conf/server.crt points to a PEM encoded certificate)

You can update `ServerName` in order to use the real domain name of your server.
You can keep the `SSLCertificateFile` as the update script will override the files when a new certificate is available.

## Automatic certificate renewals

Certbot has 2 parameters which we are going to use: `--pre-hook` and `--post-hook`.

### Create a 'copy certificates' script
Create the following script on your server (I personnaly created it in `/opt/electriccloud/electriccommander/apache/conf`):

Do not forget to update `domain.name` with the value of your domain name!

```bash
#!/bin/sh

set -e

# make sure copied files will inherit owner+group from parent
chmod g+s /opt/electriccloud/electriccommander/apache/conf

# Make sure the certificate and private key files are
# never world readable, even just for an instant
umask 077

# copy letsencrypt files
cp "/etc/letsencrypt/live/domain.name/fullchain.pem" "/opt/electriccloud/electriccommander/apache/conf/server.crt"
cp "/etc/letsencrypt/live/domain.name/privkey.pem" "/opt/electriccloud/electriccommander/apache/conf/server.key"

chmod 400 "/opt/electriccloud/electriccommander/apache/conf/server.crt" "/opt/electriccloud/electriccommander/apache/conf/server.key"
```

### Edit Certbot configuration to use this script

Edit the LetsEncrypt configuration page :

`/etc/letsencrypt/renewal/domain.name.conf`

Add these 2 lines:

``` properties
pre_hook = sudo systemctl stop commanderApache.service; sleep 5
post_hook = /opt/electriccloud/electriccommander/apache/conf/renew_certificates.sh; systemctl start commanderApache.service
```

## Test the renewal
You can now test the renewal of the certificates by doing a dry-run:

`sudo certbot renew --dry-run`

If this command is successful, your Flow  server should know use a LetsEncrypt certificate.

By default, the certificate is valid for 3 months.

Ubuntu will automatically execute the renewal action before the renewal. No manual action is needed anymore.
