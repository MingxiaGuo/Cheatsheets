# Kubernetes

* PDF: https://sematext.com/kubernetes-cheat-sheet/
* WEBSITE: https://kubernetes.io/
* DOCUMENTATION: https://kubernetes.io/docs/home
* Command line tool (kubectl): https://kubernetes.io/docs/reference/kubectl/
* kubectl Cheat Sheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
* kubectl for Docker Users: https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/
* kubectl-commands: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands
* JSONPath Support: https://kubernetes.io/docs/reference/kubectl/jsonpath/

## Client Configuration

* Setup autocomplete in bash; bash-completion package should be installed first
```
source <(kubectl completion bash)
```

* View Kubernetes config
```
kubectl config view
```

* View specific config items by json path
```
kubectl config view -o jsonpath='{.users[?(@.name == "k8s")].user.password}'
```

* Set credentials for `foo.kuberntes.com`
```
kubectl config set-credentials kubeuser/foo.kubernetes.com --username=kubeuser --password=kubepassword
```

* Set active namespace
```
kubectl config set-context --current --namespace=namespace_name
```

## Viewing, Finding resources

* Get a complete list of supported resources

  ```
  kubectl api-resources
  ```

* Get a detailed description of that resource

  ```shell
  kubectl explain <resource>
  kubectl explain pods
  ```

* List all services in the namespace

  ```shell
  kubectl get node
  kubectl get services # List all services in the namespace
  kubectl get pods -o wide --all-namespaces # List all pods in all namespaces in wide format
  kubectl get pods -o json # List all pods in json (or yaml) format
  kubectl get services --sort-by=.metadata.name # List services sorted by name
  kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' # List pods sorted by restart count
  
  kubectl get VirtualMachine -o custom-columns="Name:.metadata.name,Namespace:.metadata.namespace" -A # List name and namespcae of the resource VirtualMachine
  
  ```
* Describe resource details (node, pod, svc)

  ```shell
  kubectl describe nodes <node-name>
  kubectl describe pod <pod-name> -n <namespace>
  ```
* Rolling update pods for frontend-v1
```
kubectl rolling-update frontend-v1 -f frontend-v2.json
```

* Scale a replicaset named 'foo' to 3
```
kubectl scale --replicas=3 rs/foo
```

* Scale a resource specified in "foo.yaml" to 3
```
kubectl scale --replicas=3 -f foo.yaml
```

* Execute a command in every pod / replica
```bash
for i in 0 1; do kubectl exec foo-$i -- sh -c 'echo $(hostname) > /usr/share/nginx/html/index.html'; done
```



```
kubectl edit deployments <pod-name> -n genctl
```



## Manage Resources

* Get documentation for pod or service
```
kubectl explain pods,svc
```

* Create resource(s) like pods, services or daemonsets
```
kubectl create -f ./my-manifest.yaml
```

* Apply a configuration to a resource
```
kubectl apply -f ./my-manifest.yaml
```

* Start a single instance of Nginx
```
kubectl run nginx --image=nginx
```

* Create a secret with several keys
```
cat <<EOF | kubectl create -f -
apiVersion: v1
kind: Secret
metadata:
 name: mysecret
type: Opaque
data:
 password: $(echo "s33msi4" | base64)
 username: $(echo "jane"| base64)
EOF
```

* Delete a resource
```
kubectl delete -f ./my-manifest.yaml
```

## Monitoring & Logging

* Deploy Heapster from Github repository
```
kubectl create -f deploy/kube-config/standalone/
```

* Show metrics for nodes
```
kubectl top node
```

* Show metrics for pods
```
kubectl top pod
```

* Show metrics for a given pod and its containers
```
kubectl top pod pod_name --containers
```

* Dump pod logs (stdout)
```
kubectl logs pod_name
```

* Stream pod container logs (stdout, multi-container case)
```
kubectl logs -f pod_name -c my-container
```

## Interacting with running pods

* Run command in pod
```
kubectl exec pod_name -- command_name
```

* Run command in pod with multiple containers
```
kubectl exec pod_name -c container_name -- command_name
```

* Get terminal of pod
```
kubectl exec -it pod_name /bin/sh
```

* Get terminal of a container running in pod with multiple containers
```
kubectl exec -it pod_name -c container_name /bin/sh
```
