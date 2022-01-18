# Deploy a Production Ready Karmada cluster

This playbook will install Karmada and register a set of clusters to the karmada control plane.
Note this has been tested with karmada v1.0.0 and v0.10.1. It may not work with other releases.

## Quick Start

To deploy the cluster you can use :
```ShellSession
# Install dependencies from ``requirements.txt``
sudo pip install -r requirements.txt

# Review and change parameters under ``inventory/group_vars``
cat inventory/group_vars/karmada.yml

# See an example of the inventory(./inventory/host.ini) and customize it for your clusters.
Here is a minimum inventory:
host-cluster ansible_ssh_host=192.168.1.1 etcd_member_name=etcd1 
member1 ansible_ssh_host=192.168.1.2  member_cluster_name=member1 sync_mode=pull
member2 ansible_ssh_host=192.168.1.3  member_cluster_name=member2 sync_mode=push
member3 ansible_ssh_host=192.168.1.4  member_cluster_name=member3 sync_mode=push
member4 ansible_ssh_host=192.168.1.5  member_cluster_name=member4 sync_mode=pull

[karmada-etcd]
host-cluster
[karmada-controlplane]
host-cluster
[karmada-member]
member1
member2
[add-member]
member3
[del-member]
member4

# You can run the playbook as follows to install karmada:
ansible-playbook -i inventory/hosts.ini -e @./inventory/group_vars/karmada.yml install-karmada.yml

# Reset karmada:
ansible-playbook -i inventory/hosts.ini -e @./inventory/group_vars/karmada.yml reset-karmada.yml

# Add member cluster:
ansible-playbook -i inventory/hosts.ini -e @./inventory/group_vars/karmada.yml add-member.yml

# Delete member cluster:
ansible-playbook -i inventory/hosts.ini -e @./inventory/group_vars/karmada.yml del-member.yml

## Roadmap

- [x] deploy ha karmada control plane
- [ ] use kubectl-karmada to install karmada control plane
- [ ] use karmada helm charts to install karmada control plane
- [ ] external etcd cluster support
- [ ] CI support(ansible、github、kubevirt)
