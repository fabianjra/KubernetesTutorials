# Process to install all the YAML files:

1. `kubectl get pods`: Check if there any pods running.

- **Install the application frontend 1 (voting)**:
2. `kubectl create -f voting-app-pod.yaml`: Run the voting app.
3. `kubectl create -f voting-app-service.yaml`: Run the voting app service.
4. `kubectl get pod, svc`: Check both pod running.
5. `minikube service voting-service --url`: Check the voting service IP Address and PORT.

- **Install the redis cache**:
6. `kubectl create -f redis-pod.yaml`: Run the redis pod.
7. `kubectl create -f redis-service.yaml`: Run the redis service pod.
8. `kubectl get pod, svc`: Check all pod running.

- **Install the data base**:
9. `kubectl create -f postgres-pod.yaml`: Run the postgres pod.
10. `kubectl create -f postgres-service.yaml`: Run the postgres service pod.
11. `kubectl get pod, svc`: Check the new status.

- **Install the worker**:
12. `kubectl create -f worker-app-pod.yaml`: Run the worker pod.
13. `kubectl get pod, svc`: Check the new status.

- **Install the application frontend 2 (result)**:
14. `kubectl create -f result-app-pod.yaml`: Run the result app.
15. `kubectl create -f result-app-service.yaml`: Run the result app service.
16. `kubectl get pod, svc`: Check the new status.
17. `minikube service result-service --url`: Check the result service IP Address and PORT.


# Stop using pods and user DEPLOYS:

- Delete exiting pods, services and others to clean all.
- Create the YAML deploy files for each pod.
- Install the deployments:

- **Install the application deploy frontend 1 (voting)**:
1. `kubectl create -f voting-app-deploy.yaml`: Create the deploy.
2. `kubectl create -f voting-app-service.yaml`: Create the service.
3. `kubectl get deployment`: Check if deploys running.

- **Install the redis and data base deploy**:
4. `kubectl create -f redis-deploy.yaml`: Create the deploy.
5. `kubectl create -f redis-service.yaml`: Create the service.

4. `kubectl create -f posrtgres-deploy.yaml`: Create the deploy.
5. `kubectl create -f posrtgres-service.yaml`: Create the service.

6. `kubectl get deployment`: Check if deploys running.
7. `kubectl get pods,svc`: Check if pods and service running.

- **Install the worker deploy**:
8. `kubectl create -f worker-app-deploy.yaml`: Create the deploy. WORKER DOES NOT USE SERVICE.

- **Install the result application deploy frontend 2 (result)**:
9. `kubectl create -f result-app-deploy.yaml`: Create the deploy.
10. `kubectl create -f result-app-service.yaml`: Create the service.
11. `kubectl get deployments,svc`: Check if deploys and service running.

- **Check and test**:
12. `minikube service voting-service --url`: Check the voting service IP Address and PORT.
13. `minikube service result-service --url`: Check the result service IP Address and PORT.

- **Scale up the deployments: update replicas**:
14. `kubectl scale deployment voting-app-deploy --replicas=3`: Scale up the deployment replicas. Updated to 3 Pods.


# Reference:
- Link to the original yaml definitions in github: [Source](https://github.com/kodekloudhub/example-voting-app-kubernetes)