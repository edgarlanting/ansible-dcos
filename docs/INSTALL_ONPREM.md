# Steps for DC/OS installation with Ansible On-Premises

With the following guide, you are able to install a DC/OS cluster on premises. You need the Ansible tool installed.
On MacOS, you can use [brew](https://brew.sh/) for that.

```shell
$ brew install ansible
```

Execute `ssh-add {keypair}.pem` to be able to access your cluster nodes via SSH

Copy `./hosts.example.yaml` to `./hosts.yaml` and fill in the public IP addresses of your cluster so that Ansible can reach them and additionally set for the variables `bootstrap_ip` and `master_list` the private/internal IP addresses for cluster-internal communication. For example:

```
---
# Example for an ansible inventory file
all:
  children:
    bootstraps:
      hosts:
        # Public IP Address of the Bootstrap Node
        1.0.0.1:
    masters:
      hosts:
        # Public IP Addresses for the Master Nodes
        1.0.0.2:
    agents:
      hosts:
        # Public IP Addresses for the Agent Nodes
        1.0.0.3:
        1.0.0.4:
    agent_publics:
      hosts:
        # Public IP Addresses for the Public Agent Nodes
        1.0.0.5:
  vars:
    # IaaS target for DC/OS deployment
    # options: aws, gcp, azure or onprem
    dcos_iaas_target: 'onprem'

    # Choose the IP Detect Script
    # options: eth0, eth1, ... (or other device name for existing network interface)
    dcos_ip_detect_interface: 'eth0'

    # (internal/private) IP Address of the Bootstrap Node
    dcos_bootstrap_ip: '2.0.0.1'

    # (internal/private) IP Addresses for the Master Nodes
    dcos_master_list:
      - 2.0.0.2

    # DNS Resolvers
    dcos_resolvers:
      - 8.8.4.4
      - 8.8.8.8

    # DNS Search Domain
    dcos_dns_search: 'None'

    # Internal Loadbalancer DNS for Masters (only needed for exhibitor: aws_s3)
    dcos_exhibitor_address: 'masterlb.internal'
```

The setup variables for DC/OS are defined in the file `group_vars/all`. Copy the example file, by running:

```shell
$ cp group_vars/all.example group_vars/all
```

The now created file `group_vars/all` is for configuring DC/OS. You have to fill in the variables that match your preferred configuration. The variables are explained within the file.

To check that all instances are reachable via Ansible, run the following:

```shell
$ ansible all -m ping
```

Finally, you can install DC/OS by applying the Absible playbook:

```shell
$ ansible-playbook plays/install.yml
```
