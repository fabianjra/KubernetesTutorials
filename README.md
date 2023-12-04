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
	* kubectl run nginx --image=nginx -> Deploy a Docker container by creating a pod. First creates a pod automatically and deploys an instance of the Nginx docker iamge.
	* kubectl get pods -> See all the pods.
	kubectl describe pod nginx -> Get information about tht pods.
	* kubectl get pods -o wide -> Additional inforamcion, like IP.
	* kubectl delete pod `podname` -> Delete a single pod.


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

# PODS with YAML:

- YAML files are used as inputs for POD creations.
 - Top/Root levels of properties, case sensitive, required (with examples):

```
apitVersion: v1 #String
kind: Pod #String
metadata: #Dictionary
	name: myapp-pod #String
	labels: #Dictionary
		app: myapp

spec: #Dictionary
	containers: #List/Array
		- name: nginx-container
			image: nginx
```

- Commands (-f is for the file name):
	* kubectl create -f pod-definition.yaml -> Create the pod with the Yaml definitions.
	* kubectl apply -f pod.yaml -> Apply is another way to create the pod
	* kubectl get pods -> See the pods
	* kubectl describe pod myapp-pod -> More information about the pod.
	* cat pod.yaml -> Review the yaml file to verify children and sitax is correct.
	* kubectl run `pod name` --image=`image name` --dry-run=client -o yaml ->  see the YAML file.
	* kubectl run `pod name` --image=`image name` --dry-run=client -o yaml > `file name`.yaml -> Create the file using the YAML information.
	* kubectl create -f redis.yaml -> Create the pod using the YAML file.
	* vi `file name`.yaml -> Edit the YAML file.
	* kubectl apply -f redis.yaml -> Apply changes on a file.

- Configurar VS Code para trabajar con YAML en Kubernetes:
	* En los ajustes de la extension YAML, ir al archivo settings.json del schema. Agregar la linea de codigo de soporte de YAML para kubernetes y deberia quedar de la siguiente manera:
```
    "yaml.schemas": {
        "kubernetes": "*.yaml"
    }
```

# Controllers:
- The brain behind kubernetes.
- Replication controller:
	* Ensures that the specified number of pods are running at all times.
	* Load balancing and scaling. Example: Increase number of pods or nodes, as the number of users are growing up.
	* How to create the Replication controller:
	* The important part is in the spec: Add a Template, which is exactly what we used to create the Pod.
```
apitVersion: v1
kind: ReplicationController
metadata:
	name: myapp-rc
	labels: 
		app: myapp
		type: front-end

spec: 
	template: #Pod Template
		metadata: #Child of template
			name: myapp-pod 
			labels:
				app: myapp
				type: front-end
		spec:
			containers:
				- name: nginx-container
				  image: nginx

	replicas: 3 #Replica count
```

- Commands for replication controoler:
	* kubectl create -f rc.definitions.yaml -> Create the replica controller based in a YAML file.
	* kubectl get replicationcontroller -> Get the information and number of replication controllers.

- Replica set:
	* New way to do the replication controller.
	* Creation of Replica set YAML definitions:
	* Selector is the mosr important difference, it is used to manage pods out from the replica set.

```
apitVersion: apps/v1 #First difference
kind: ReplicaSet
metadata:
	name: myapp-replicaset
	labels: 
		app: myapp
		type: front-end

spec: 
	template: #Pod Template
		metadata: #Child of template
			name: myapp-pod 
			labels:
				app: myapp
				type: front-end
		spec:
			containers:
				- name: nginx-container
				  image: nginx

	replicas: 3 #Replica count
		matchLabel:
			type: front-end

	#Replica set, require a selector definition:
	selector: #What Pod under it
```

- Commands for replica set:
	* kubectl create -f replicaset-definition.yml -> Create the replica set based in a YAML file.
	* kubectl get replicaset -> Get the information and number of replica set.
	* kubectl delete replicaset myapp-replicaset -> Delete also all underlying PODs.
	* kubectl replace -f replicaset-definition.yml -> Update the replcia set definition if a property changed. Example: cahnge the number of replcias.
	* kubectl scale --replicas=6 replicaset-definition.yml -> Update the replicas.
	* kubectl scale --replicas=6 replicaset myapp-replicaset -> Another way to update the replicas.