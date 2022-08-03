## Aula 2 - Kubernetes

**Principais ferramentas do mercado:**

- kubeadm
- RKE/RKE2
- MicroK8s
- K3s

**Kubernetes as a service:**

*Cloud providers*
- GKE - GCP
- EKS - AWS
- AKS - Azure

**Localmente**

*testes e poc*

- minikube
- kind
- k3d

--
### **instalação local usando k3d**

*Usando wsl com docker e kubectl instalado*

```
curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
```

Criando cluster com 3 server e 3 workerloads
```
k3d cluster create meucluster --servers 3 --agents 3
k3d cluster list
kubectl get nodes

```


kubectl api-resources
 pods                              po           v1                                     true         Pod

kubectl apply -f pod.yaml

kubectl get pods -o wide
NAME    READY   STATUS    RESTARTS   AGE     IP          NODE                     NOMINATED NODE   READINESS GATES
mypod   1/1     Running   0          2m23s   10.42.4.3   k3d-meucluster-agent-2   <none>           <none>


kubectl describe pod mypod

*Acessando o pod
```
kubectl port-forward pod/mypod 8080:80
```

--

utilizando labels
kubectl get pods -l app=green

**ReplicaSet**

weslley_barboza@n1045:~/aulas/02-kubernetes/pods$ kubectl apply -f replicaset.yaml
replicaset.apps/meureplicaset created
weslley_barboza@n1045:~/aulas/02-kubernetes/pods$ kubectl get replicaset
NAME            DESIRED   CURRENT   READY   AGE
meureplicaset   1         1         0       7s
weslley_barboza@n1045:~/aulas/02-kubernetes/weslley_barboza@n1045:~/aulas/02-kubernetes/pods$ kubectl get replicaset -o wide
NAME            DESIRED   CURRENT   READY   AGE   CONTAINERS   IMAGES                          SELECTOR
meureplicaset   1         1         1       37s   web          fabricioveronez/web-page:blue   app=web

testando resiliencia, deletando o pod, irá ser criado automaticamente pois esta sendo gerenciado pelo replicaset

gerenciando troca de versão
com 

**Deployment**


kubectl get replicaset
NAME                      DESIRED   CURRENT   READY   AGE
meudeployment-9fb78bf46   0         0         0       5m20s
meudeployment-b9548cb65   10        10        10      4m35s

fazendo rollback
```
kubectl rollout history deployment meu deployment
kubectl rollout undo deployment meudeployment
```

kubectl get replicaset
NAME                      DESIRED   CURRENT   READY   AGE
meudeployment-9fb78bf46   10        10        10      7m3s
meudeployment-b9548cb65   0         0         0       6m18s


## **Services**

Acessando os pods sem precisar fazer o port-forward

- ClusterIP *ponto de acesso aos pods internamente*
- NodePort *Expõe externamente 30000-32767> utiliza uma porta aleatorio, será exposto em todos os nós do cluster (baremetal-onpremisse)*
- LoadBalancer *Expoe externamente, utiliza o serviço do cloud provider on premisse utilizando ferramenta como metalLB*

Criando services
incluir no manifesto


k3d cluster create meucluster --servers 3 --agents 3 -p "30000:30000@loadbalancer"


forward
kubectl port-forward service/postgres 5432:5432