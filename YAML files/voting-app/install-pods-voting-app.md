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