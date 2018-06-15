
## Tool?
- Tool deploys  k8s cluster by just using ip, & user credentials for nodes.
- Also tool will add new node easily to the existing cluster
- Handles multiple os platforms
 
## Why ansible playbooks & value adds?
-	Ansible is open source, simple, powerful & agentless.
-	Ansible is the most widely used app deployment,  configuration management & orchestration dev-ops tool  in the industry.
-	Easy install  ‘x’ nodes in parallel  which saves manual efforts.
-	During development/testing phase if our cluster goes down for any reason, nodes  can be recovered via playbooks instead of manual, also can remove stale nodes + adds new nodes. 
-	Current playbooks can be extended to build baas workflow & also upgrade baas software seamlessly.
-	Helps in scaled environment.
-	Manual effort saved.

## Ansible playbooks YAML:
-	k8s_cluster.yml   ->  Deploys  ‘x’ node cluster without any manual intervention.
-	k8s_node_add.yml -> Adds new node easily to the existing cluster.
-	k8s_node_delete.yml  - > WIP
-	Baas_work_flow.yml –>  future

## How to use?
-	One time setup on your ansible controller, where ansible play-books are executed [Any linux host/VM].
1.	yum install git -y
2.	yum install ansible -y
3.	cd ~
4.	git clone https://github.com/gayathrl/ng-ansible-playbooks.git

#### Deploy k8s cluster ###########
1.	Edit hosts file @ ~/ng-ansible-playbooks/baas-ansible-deployment/hosts  and provide your node details as shown below:
```
[root@centos72-1-gayathrl ~]# cat /root/ng-ansible-playbooks/baas-ansible-deployment/hosts  
[k8smaster]
10.61.131.97

[k8smaster:vars]
user = root
passwd = xxxxxx

[k8sworkers]
10.61.131.95

[k8sworkers:vars]
user = root
passwd = xxxxxx
```
2.	Execute ansible playbook:
```
[root@centos72-1-gayathrl baas-ansible-deployment]# cd /root/ng-ansible-playbooks/baas-ansible-deployment
[root@centos72-1-gayathrl baas-ansible-deployment]# ansible-playbook k8s_cluster.yml
```

3.	K8s cluster should be ready in just < 15min.


#### Add new node to the k8s cluster ####################
1.	Edit hosts file @/root/ng-ansible-playbooks/baas-ansible-deployment/hosts  and provide your node details as shown below:
```
[root@centos72-1-gayathrl ~]# cat /root/ng-ansible-playbooks/baas-ansible-deployment/hosts
[k8smaster]
10.61.131.97

[k8smaster:vars]
user = root
passwd = xxxxxx

[k8sworkers]
10.61.131.95

[k8sworkers:vars]
user = root
passwd = xxxxxx

[newnode]
10.61.131.126

[newnode:vars]
user = root
passwd = xxxxxx
```

2.	Execute ansible playbook:
```
[root@centos72-1-gayathrl baas-ansible-deployment]# cd /root/ng-ansible-playbooks/baas-ansible-deployment
[root@centos72-1-gayathrl baas-ansible-deployment]# ansible-playbook k8s_cluster.yml
```

NOTE:
 - For non root users:
     - refer sample hosts file named "hosts_non_root_user_sample" in the code base
     - Invoke you ansible playbook as shown below:
     ```
     [root@centos72-1-gayathrl baas-ansible-deployment]# ansible-playbook k8s_cluster.yml -b -K
     SUDO PASSWORD: <enter your sudo password [asks only once]>
     ```
 

3.	New node gets added to k8s cluster.


