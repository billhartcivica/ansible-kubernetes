# Kubernetes Single-Master Cluster (Ansible)

## Ansible files for prepping a group of machines to function in a Kubernetes environment

To import a group of machines into a Kubernetes environment, target hosts must first be prepared. The host specification should be as follow:

- Base O/S: Centos 7
- Setup root user with password
- Create an entry for each host in the /etc/hosts file of the Ansible host
- Run the 'ssh-copy-id root@<host>' command to copy your SSH key from the Ansible host to the target host(s)
- Ensure each hostname/IP is in each machine's /etc/hosts file (if running Ansible as root, this can be done via the ipaddr.yml file).
- Add each hostname/IP to the /etc/hosts file on the Ansible control host.

Once the base installation is complete, the prep.yml playbook will carry out the following steps:
- User: Create the ansible user (subsequent Ansible playbook runs can use this account!)
- The sudoers file is amended so that members of the 'wheel' group can sudo without login
- The 'ansible' user is made a member of the 'wheel' and 'docker' group.
- Selinux is disabled and firewalld is stopped/disabled.
- Docker is installed, enabled and started.
- The 'inventory' file in this repo needs to be updated with the hostnames of the target cluster hosts.
- The worker nodes will be joined automatically to the master node as workers.

The above steps are carried out by the 'run.sh' script which calls the Ansible playbooks in order. Once done, shell onto the master and pick up the config file in the
/etc/kubernetes folder and copy it to the 'config' file in your local /home/username/.kube folder. Assuming you have installed kubectl on your local host, you should
now be able to issue remote kubectl commands to the finished cluster.
