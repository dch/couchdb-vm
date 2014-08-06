# ASF CouchDB VM script

This is an [ansible]-managed [playbook] used to manage the Apache CouchDB
project's Ubuntu virtual machine. The ASF Infrastructure team already use
puppet to manage the core box so this playbook focuses on the couchdb
application layer only.

NB ideally no changes should be made directly on the server, everything
should be managed through this script/repository.

## Login via ssh to the VM

The point of this repo is that most committers will likely not need access to
the vm at all, but if you do:

- update your `ssh` public key on https://id.apache.org/
- file a [jira] ticket for project INFRA requesting access to `couchdb-vm.apache.org`
- prod one of the PMC members to +1 the ticket on your behalf
- if sudo is required (and it probably shouldn't be) note this in the ticket
- sudo users will need to set up [opie], a one-time password tool, prior

## Bootstrapping

Obtain logins as above, ssh in and `sudo -s`, with [opie] to gain root privileges.

    apt-get install python-software-properties -y
    add-apt-repository ppa:couchdb/stable -y
    apt-get update -y
    apt-get install -y ansible
    rm -rf /tmp/.ansible && ansible-pull -v -i localhost, \
        --url https://github.com/dch/couchdb-vm.git \
        --directory=/tmp/.ansible --purge

This will install ansible itself, pull down this repo, and set up a `cron`
task to run every 15 minutes pulling the repo and making any needed changes.

[opie]: http://docs.asfcloud.com/committer/opie
[ansible]: http://ansible.com/
[playbook]: http://docs.ansible.com/playbooks.html
[jira]: https://issues.apache.org/jira/browse/INFRA
