### Ansible Playbooks to set up HA cluster on Premise Ubuntu

## Prerequisites:

   1) Install ansible on your manager machine
            
            `apt-get install python3 python3-pip`
            `pip3 install ansible`

   2) Set up ssh access to workers. You can either set up with ssh keys or with username and password     
           
   3) Make sure that ssh connection is possible between workers
 
   4) update inventory/cluster with masters, workers and with desired k8s_version
   
## Prepare Environment:

 `ansible-playbook -i inventory/cluster playbooks/configure-nodes.yml`
 
 This mainly installs kubeadm, kubectl, kubelete on K8s-nodes and other required packages.
 
 ## Note: Please check kubeadm_verison you specify in the vars is actaully availble in the list
 `curl -s https://packages.cloud.google.com/apt/dists/kubernetes-xenial/main/binary-amd64/Packages | grep Version | awk '{print $2}'`
 
 ## Launch HA cluster:
 
 `ansible-playbook -i inventory/cluster playbooks/launch-has-cluster.yml`
 
 This creates a ha cluster with stacked control plane nodes, you can read more about that @ https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/#stacked-control-plane-and-etcd-nodes
 
 ## Add new control plane node:
 
  update inventory file with appropriate k8s-control-plane-primary and k8s-control-plane-members
  
 `ansible-playbook -i inventory/cluster playbooks/add-controlplane.yml`
 
 ## Add new worker node
 
  update inventory file with appropriate k8s-control-plane-primary and k8s-worker-nodes
 
 `ansible-playbook -i inventory/cluster playbooks/add-workernode.yml`
 
 ## Reset cluster
 
 `ansible-playbook -i inventory/cluster playbooks/reset-cluster.yml`      