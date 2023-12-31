# General notes:
- Redis: Key Value storage.

# Kubernetes:

- We cannot deploy containers in Kubernetes. Smallest object in a Kubernetes cluster is a Pod.

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
	* Selector is the most important difference, it is used to manage pods out from the replica set.

```
apitVersion: apps/v1 #First difference
kind: ReplicaSet
metadata:
	name: myapp-replicaSet
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
	* kubectl describe replicaset myapp-replicaset -> More information about the replica set.
	* kubectl delete replicaset myapp-replicaset -> Delete also all underlying PODs.
	* kubectl replace -f replicaset-definition.yml -> Update the replcia set definition if a property changed. Example: cahnge the number of replcias.
	* kubectl scale --replicas=6 replicaset-definition.yml -> Update the replicas.
	* kubectl scale --replicas=6 replicaset myapp-replicaset -> Another way to update the replicas.
	* kubectl edit replicaset myapp-replicaset -> Edit the replica set configuration in YAML structure (temporal file).
	* kubectl scale rs <rs name> --replicas=5 -> Update the replicas.


# Deployments:

- Jerarquia de los deploymentos: Deployment -> ReplicaSet -> Pods -> Containers
- Al crear un deployment, automaticamente se crea el replicaset y los Pods.
- Commands for Deployments:
	* `kubectl create -f deployment-definition.yaml` -> Create a deployment
	* `kubectl get deployments` -> Get the deployments
	* `kubectl get replicaset` or `kubectl get rs` -> Get the Deployment's replicaset
	* `kubectl get pods` -> Get the Replicaset's pods.
	* `kubectl get all` -> Get the deployment, replica sets and pods.
	* `kubectl create deployment httpd-frontend --image=http:2.4-alpine --replicas=3` -> Crear un deploymento con nombre especifigo: "httpd-frontend", imagen: "http:2.4-alpine" y replicas: 3.
	* `kubecetl apply -f <yaml file name>` -> Update changes when modify the yaml file.
	* `kubecetl set image deployment/<deploy name> nginx=nginx:1.9.1` -> Update the image
	* `kubectl rollout undo deployment/<deploy name>` -> Rollback the deployment.
	* `kubectl edit deploy <deploy name>` -> Edit the Deployment.

- **Rollout**: Create a new deployment revision.
- Rollout commands:
	* `kubectl rollout status deployment/<app name>` -> Status of the rollout.
	* `kubectl rollout history deployment/<app name>` -> Revisions and history information.
	* `kubectl create -f <app name>.yaml --record` -> Instructs Kubernetes to record the cause of change. So will keep tha change for the rollout history.
	* `kubectlset image deploy <deploy name> nginx=nginx:1.18-perl --record` -> Update the image of the deployment.

- 2 Types of Deployment Strategy:
	1. Recreate: Destroy all and then create.
	2. Rolling Update (default): Not destroy all at once. Instead, delete one by one.


# Networking in Kubernetes:

- IP Addres is assigned to a POD. It gets its own interal IP.
- Cluster networking: Different tools can assing IP addresses to the node and interla pods to communicate each other.
- Kubernetes Services: It's used to communicate applicaction with other applications or users. Example: Group of backend to a group of front end applications.
- Services inside a node is in charge of connect the pod inside the node with the external IP addres from the node, using a port.
- Node port service: redirect the call to the correct pod.

- **Services types**:

	* **NodePort**: Through a port we can access a POD from outside the node, using ports. PORT -> Service | TargetPort: POD.
		1. Definition YAML:
		```
		apiVersion: v1
		kind: Service

		metadata:
		  name: myapp-service

		spec: 
		  type: NodePort #Type of node service.
		  ports:
		    - targetPort: 80 #POD
			  port: 80 #Service
			  nodePort: 30008 #Outside node exposition to public. We ingress the node from here.

		  selector: #To link the service to the pod.
		    #Metadata Labels goes here:
			app: myapp
			type: front-end
		```
		2. Service commands:
			* `kubectl create -f <app name>.yaml` -> Create the service.
			* `kubectl get service` -> Get service information.
			* `kubectl describe service` -> More service information.
			* `curl http://192.168.1.2:30008` -> Example to acces the pod from and external IP address with port. 
			* `minikube service <service name> --url` -> Show the IP service available to access it.


	* **ClusterIP**: We can have a gruop of Front-end Pods, a group of Back-end Pods and a group of Redis Pods. A Service is in between of each group to connect each other, forming a line of connections.
		1. Enpoints: Basically the each POD has an endpoint.
		2. Front-end Group -> service -> Back-end Group -> service -> Redis Group
		3. Definition YAML:
		```
		apiVersion: v1
		kind: Service
		metadata:
		  name: back-end

		spec: 
		  type: ClusterIP #Type of node service. It's de default value.
		  ports:
		    - targetPort: 80 #Where the backend is expose.
			  port: 80 #Service expose.

		  selector: #To link the service to the pod.
		    #Metadata Labels goes here:
			app: myapp
			type: front-end
		```

	* **LoadBalancer**: This works only with supported work platforms.
		1. Definitions are exactly the same as ClusterIP, except for -> type: Loadbalancer


# Microservices:

- Commandos for docker:
	* `docker run -d --name=redis redis` -> Start the redis container.
	* `docker run -d --name=db postgres:9.4` -> Start the data base.
	* `docker run -d --name=vote -p 5000:80 voting-app` -> Start the web server.
	* `docker run -d --name=result -p 5001:80 result-app` -> Web UI.
	* `docker run -d --name=worker worker` -> Start the worker.

- Links: Connect containers together.
	* change the command to add: `--link redis:redis`. Example: `docker run -d --name=vote -p 5000:80 --link redis:redis voting-app`
	* changed and new command: * `docker run -d --name=result -p 5001:80 --link db:db result-app`
	* Link the worker with redis and db. Changed to: `docker run -d --name=worker --lnik db:db --link redis:redis worker`


# Azure:

- AKS (Azure kunernetes service):
	1. Add a cluster (create kubernetes cluster)
	2. Go to cloud shell
	3. Create credentials with command (watch documentation).
	4. Clone the repo, where you have you YAML files to store them and create pods.
	5. Run YAML files (pods or deploys, services, workers, apps). Example: kubectl create -f <definitions-file>.yaml


# References:

- Link to kubeadm installation instructions: [Source](https://kubernetes.io/docs/setup/independent/install-kubeadm)