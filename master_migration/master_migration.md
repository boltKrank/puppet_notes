# Migrating a master

## Step 1: Back up the SSL directory and clean the certificate on the master node

Note, if the server you intend to be the new Puppet master was previously used as an agent node, on your existing Puppet master, run the following command:

```
puppet cert clean <CERTNAME OF SERVER TO BE REUSED AS NEW PUPPET MASTER>
```

To start the backup, create a directory in which to store the backup files and run the following command on your Puppet master:

```
BACKUP_DIR=/tmp/backup tar -zcvf $BACKUP_DIR/puppet_ssl.tar.gz /etc/puppetlabs/puppet/ssl/
```

Note: If you don’t want to use /tmp/backup, you will need to change the BACKUP_DIR variable throughout the remaining procedures.

Use your preferred method to transfer this back up to the new Puppet master server.

## Step 2: Backup the database on the PuppetDB node

To backup the database, on the PuppetDB node, run the commands:

```
export BACKUP_DIR=/tmp/backup
chown pe-postgres:pe-postgres $BACKUP_DIR
cd $BACKUP_DIR
for db in pe-activity pe-classifier pe-orchestrator pe-puppetdb pe-rbac; do echo "Backing up $db" ; sudo -u pe-postgres /opt/puppetlabs/server/bin/pg_dump -Fc $db -f $db.backup.bin; done
```

Use your preferred method to transfer these back ups to the new Puppet master server.

## Step 3: Install PE on the new master

Install PE on the new Puppet master server. Follow the installation documentation for instructions.

## Step 4: Restore the databases and classifier data

Restore the databases, fix database permissions, and recreate database extensions on the new Puppet master. Note that commands that have <NAME> placeholders will need to be updated to reflect the correct fully qualified domain names.

Run the following commands:

```
export BACKUP_DIR=/tmp/backup
cd $BACKUP_DIR
for svc in puppet pe-puppetserver pe-puppetdb pe-console-services pe-nginx pe-activemq pe-orchestration-services pxp-agent; do echo "Stopping $svc" ; puppet resource service $svc ensure=stopped; done
for db in pe-activity pe-classifier pe-orchestrator pe-puppetdb pe-rbac; do echo "Restoring $db" ; sudo -u pe-postgres /opt/puppetlabs/server/bin/pg_restore -Cc $BACKUP_DIR/$db.backup.bin -d template1; done
/opt/puppetlabs/bin/puppet-enterprise configure
```

## Step 5: (Optional) Deactivate and clear the certificate from your old Puppet master

Warning: If your old master and new master share a certificate name you should not complete this step.
Run the following commands:

```
for cert in <OLD MASTER FQDN> <OLD PUPPETDB FQDN> <OLD CONSOLE FQDN>; do puppet node purge $cert; puppet cert clean $cert; done
```


## Step 6: Migrate your data

Migrate your configuration files, Puppet code, and Hiera data.
* Manually update settings in puppet.conf to incorporate any customizations present on the split master
* If using Code Manager or r10k, confirm the configuration is in place on the new master and deploy code. Otherwise, copy the contents of the code directory /etc/puppetlabs/code/ to the new master.
* Migrate your Hiera data
* Put your existing hiera.yaml file in /etc/puppetlabs/puppet/hiera.yaml
(Optional) If you set up a third-party certificate for your console, re-configure it according to the custom console cert documentation.

## Step 7: Configure your Puppet agents to point at the new Puppet master

Update the puppet.conf file for each agent to point it to the new Puppet master. Compile masters will need to have their certificates regenerated to include the alt_dns_names of the new Puppet master. Follow the instructions at https://docs.puppet.com/pe/latest/agent_cert_regen.html and make sure to include --allow-dns-alt-names when signing the certificate on the new master.
