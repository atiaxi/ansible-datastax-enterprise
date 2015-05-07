# How do I use this?

You'll need to change a number of files:

## inventory

You don't have to use this file, but ansible will require an inventory
file somewhere.  There are two sections, `[cassandra]` for all the
machines that will have Cassandra installed, and `[opscenter]` for the
machine that will have opscenter installed.

## group_vars/cassandra.yml

This contains the cluster name, seeds, and opscenter 'stomp address' setting.
Additionally, the multi-DC version has public IP addresses for each of the
nodes; this particular playbook setup doesn't make any EC2 API calls
so the public addresses have to be hardcoded into this file.

## private_vars/*_credentials.example.yml

You'll need to rename these to dse_credentials.yml and email_credentials.yml
and supply actual values for them.

## roles/cassandra/templates/cassandra.yaml.y2

You may have noticed that the cassandra.yml file in group_vars, above,
doesn't actually have a lot of configuration in it.  The rest of the
configuration exists in this file.  It's the default cassandra.yaml
file that ships with DSE 4.6.6 with a few changes for the small
amounts of templating currently done and a snitch appropriate to
single-region or multiregion setups.  If you want to override the
defaults, you'll need to change this file.

In the future, this file is likely to become much more templated so
you only have to make changes in one file.

# Caveats

## This is for Ubuntu

Future work may include changes for e.g. RedHat, Centos, etc.

## Java is openjdk

Datastax recommends using Oracle Java for production installs, but
openjdk is simple to install and works well for demonstration purposes.

## Make sure your security groups are set up correctly

Amazon's EC2, by default, comes with a very restrictive set of rules
determining what can and cannot connect to your machines.  Since these
playbooks do not currently make EC2 API calls, they cannot set up
security groups for you.  If your nodes cannot connect to each other,
this is likely the culprit.
