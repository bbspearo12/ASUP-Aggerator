### Postfix mail server configuration 
(assumed to be on Ubuntu 14.04 vm)

Logstash agent is supposed process and tag and ingest emails from the email server in the elastic search cluster. Postfix mail server is assumed to installed and properly configured in the production environments, however in the dev environment the following can be used as cheat sheet to install and configure it.

```
sudo apt-get update
sudo apt-get install postfix
```
During installation FQDN has to be entered. On the dev vm, we can get away with entering `localhost`.
Main configuration is stored under 
```
/etc/postfix/main.cf
```
After the installation is complete, check for the following configurations in the file
1. `mydestination` and `myhostname` are both set to `localhost`
2. `mynetworks` is set to `127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128
`

Create an virtual file that will make some fake users to unix system accounts. 
*Sample file:*

```
root@localhost	root
asup-alerts@localhost	root
```
Reload and restart postfix server
```
postmap /etc/postfix/virtual
service postfix restart
```
#### Test setup
To test our postfix mail server setup we will need a mail client. We will use `mailutils` a command line mail client, this can also be used to script some tests.

Install `mailutils`
```
apt-get install mailutils
```

Send test email
```
mail -s "Hello world - 2" asup-alerts@localhost <<< "Live long and prosper, yet again"
```
