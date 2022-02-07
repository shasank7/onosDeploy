# ONOS DEPLOYMENT On KIND CLUSTER
Step by step onos deployment into kubernetes

# Pre-requisites

- Windows powershell
- go
- helm
- kubectl
- docker


# Create a Kind Cluster with YAML file 
  ```
  kind create cluster --config kind-config.yml
```
# Setup kubeconfig
```
   kind get kubeconfig > ~/.kube/kind
   ```
   
 # Create Namespace
 ```
 kubectl create namespace micro-onos
 ```
 ### Setup helm repos
  
  ```
helm repo add cord https://charts.opencord.org
helm repo add atomix https://charts.atomix.io
helm repo add onosproject https://charts.onosproject.org
helm repo update
```

    If the above command fails, try adding one repo at a time 
  
### Installing helm charts 
  
  # Install atomix- controller
  
  ```
helm install -n kube-system atomix-controller atomix/atomix-controller
```
# Install atomix-raft-storage

  ```
 helm install -n kube-system atomix-raft-storage atomix/atomix-raft-storage
   ```
   # Install ONOS Operator
   
   ```
    helm install -n kube-system onos-operator onosproject/onos-operator
   ```
     
   # Install onos-umbrella
 ```
helm -n micro-onos install onos-umbrella onosproject/onos-umbrella
 ```
       
       
 ### Validate if all pods are ruuning 
```
kubectl get pods --all-namespaces
```
## You can attach to:

# Onos CLI pod with
```
$ kubectl -n micro-onos exec -it $(kubectl -n micro-onos get pods -l type=cli -o name) -- /bin/sh
```
* µONOS GUI at http://<server_IP>:31191


# If you are using KinD as a Kubernetes server, you will have to use a "port-forward" to access the GUI e.g.
```
$ kubectl -n micro-onos port-forward $(kubectl -n micro-onos get pods -l type=gui -o name) 8182:80
```
and then access the GUI at
* http://localhost:8182


 ## All Namespaces
 <img width="298" alt="Namespaces" src="https://user-images.githubusercontent.com/98982872/152426973-2f0f98b6-a5f1-41a8-a79b-80011a5a34d0.PNG">
 
 
 ## All pods running and ready
 
 <img width="666" alt="ALlpods" src="https://user-images.githubusercontent.com/98982872/152427083-2253468b-8a5a-4517-8259-981082599e2f.PNG">

 

 
 
