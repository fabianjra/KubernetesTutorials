# Kubernetes:

- 2 types of servers (nodes): Master and Worker.

- Cluster: Group of nodes grouped together.

- Worker Node (minion): 
	* Worker Machine, physical or virtual in which Kubernetes is installed.
	* It is where containers will be launched by Kubernetes.
	* It has the kubelet agent that is responsible for interacting with a master.

- Master: Another node with Kubernetes installed in it. Configured as Master.
	* Watches over the nodes in the cluster and is responsible for the orchestration of coninters in the worker nodes.
	* This has the kube API server and that is waht makes it a master.
	* All the information gathered from the node, are stored in a key value store on the master (etcd).
	* It has the controller and the scheduler.

- Kubernes install include the following:
	* API Server: Frontend for Kubernetes (Users, management devices, CLI).
	* etcd Service: Distributed reliable key value store used by kubernetes to store all data used to manage the cluster. Responsible for implementing locks within the cluster to ensure that there are no conflicts between the masters.
	* kubelet Service: Agent that runs on each node in the cluster. Responsible for making sure that the containers are running on the nodes as expected.
	* Container Runtime: The underlying software that is used to run containers (docker).
	* Controller: The brain behind orchestration. Responsible for noticing and responding when nodes, containers or end points goes down. Make decisions to bring up new containers.
	* Scheduler: Responsible for distributing work or containers across multiple nodes.

- kubectl -> Kube command line tool (kube control):
	* kubectl run hello-minikube: This command is used to deploy an application on the cluster.
	* kubectl cluster-info: This command is used to view information about the cluster.
	* kubectl get nodes: This commando is used to list all the nodes part of the cluster.


# Pods:

- This is a single instance of an application. It's the smallest object that you can create in Kubernetes.
- Pods usually have a 1 to 1 relationship with containers running your application. To scale up, you create new Pods. To scale down, you delete existing pod. You DO NOT add additional containers to an existing pod to sacale your application.
- A single pod can have multiple containers except for the fact that they are usually not multiple containers of the same kind.
- A Pod is inside the node (server worker) and inside the pod, there are application containers.
- Kubernetes concepts: [URL](https://kubernetes.io/docs/concepts/)
- Pod overview: [URL](https://kubernetes.io/docs/concepts/workloads/pods/)

- Terminal kubectl commands:
	* kubectl run nginx --image=nginx: Deploy a Docker container by creating a pod. First creates a pod automatically and deploys an instance of the Nginx docker iamge.
	* kubectl get pods: See all the pods.
	kubectl describe pod nginx: Get information about tht pods.
	* kubectl get pods -o wide: Additional inforamcion, like IP


# YAML:

- It is used to represent configuration data.
- Dictionary: Unordered collection.
- Lis: Ordered collection.

- Key value pair:
```
Fruit: Apple
Vegetable: Carrot
Liquid: Water
```

- Array / List:
```
Fruit:
- Orange
- Apple
- Banana

Vegetables:
- Carrot
- Cauliflower
- Tomato
```

- Dictionary / Map (spaces are important to create childs):
```
Banana:
 calories: 105
 Fat: 0.4 g
 Carbs: 27 g

Grapes:
 Calories: 62
 Fat: 0.3 g
 Carbs: 16 g
```

- Key value / Dictionary / Lists
```
Fuits:
 - Banana:
	Caloires: 105
	Fat: 0.4 g
	Carbs: 27 g

 - Grape:
	Calories: 62
	Fat: 0.3 g
	Carbs: 16 g
```

```
Payslips:
 - 
	Month: May
	Wage: 4000
 -
	Month: June
	Wage: 4500
 -
	Month: July
	Wage: 5300
```