# Apuntes

---

1. **Definicion de kubernets:** es una herramienta de orquestacion de contenedores que permite reiniciar contenedores caidos automaticamente caidos en produccion y reemplazarlo.
cuando un contenedores tiene mucho trafico permite el balanceador de carga(load balancer) distribuye las peticiones en los distintos servidores.
K8s automaticamente genera replicas para el escalado horizontal creando mas servidores en donde se distribuiran las peticiones.

**Beneficios de k8s:**

1. easy rollback
2. No esta restringido a ningun proveedor de la nube
3. configuracion automaticamente configura la infraestructura(load balancers etc..)
4. aumenta la velocidad de despliegue.
5. permite construir aplicaciones complejas(microservicios)

## Componentes de kubernets

1. **Cluster**: Un clúster de Kubernetes es un conjunto de máquinas de nodos que ejecutan aplicaciones en contenedores.

   **Componentes de un cluster**:

   - pods
   - Services
   - Ingress
   - Deployments
   - ReplicaSets
   - Secrets
   - ConfigMap
  **NOTA** las forma en la que se definen los recursos(componentes) en un cluster de kubernets en un archivo con extension .yml

2. **Worker node (maquina virtual)**: al menos se debe tener uno, pero se pueden tener varios(worker node). se pueden ver como computadoras que ejecutan un contenedor.

3. **Pod**: son los que envuelven a los contedores para ser ejecutados. los pods son la unidad minima de kubernets.Un pod pods puede contener multiples contenedores, lo ideal es tener un contenedor en un pod.

Un pod de Kubernetes es un conjunto de uno o varios contenedores de Linux® y constituye la unidad más pequeña de las aplicaciones de Kubernetes. Puede estar compuesto por un solo contenedor, en un caso de uso común, o por varios con conexión directa, en un caso de uso avanzado.

cluster **>** nodos(worker nodes) **>** pods **>** contenedores(ideal solo 1)

1. **kubelete**: getiona los pods(contenedores) dentro del worker node y ademas se comunica con el master node

2. **Kube-proxy**: permite la comunicacion en red(network comunication) por fuera y dentro del worker node

3. **Master node**: it contains a bunch of differents things, we can have one or many master node, it allows to mantaining the worker nodes and the pods working inside the working node.Contiene el API service que se comunica con el kubelete de los worker nodes y ademas mantiene la comunicacion con el cli de kubernets (kubectl), que es la interfaz que nos permite comunicarnos con el master node que a su vez se comunica con los worker nodes

**NOTA**: **kubectl** es el cli de k8s que nos permitira comunicarnos con el cluster(master node), permitiendonos indicarle que hacer, por ejemplo, le podemos indicar al master node que cree un recurso.

---

## Instalar micro-k8s

<https://ubuntu.com/kubernetes/install> (seleccionar linux)

## Anadir alias para MicroK8s

sudo snap alias microk8s.kubectl kubectl

## ver alias con snap : snap aliases

### COMANDOS DE KUBECTL

- sudo systemctl enable docker
- sudo systemctl start docker
  
- kubectl get all/[name] -n [namespace](retorna todo los recursos del cluster de kubernets, si no especifica un namespace hace referencia a "default namespace")
- kubectl get pods (retorna todos los pods)
- kubectl get deployments (retorna todos los deployments)
- kubectl get namespaces
- kubectl describe [name-pod] (describe un pod especifico)
- kubectl logs [name-pod] (debugg un pod)
- kubectl delete [resorce-name(deployment e.i)] [name-resorce] (eliminar un recurso) ejemplo: kubectl delete service mongo-srv
- kubectl delete all --all (elimina todo)
- mkubectl apply -f . ==> (si se coloca . se aplica para todo los archivos yaml, si se quiere un archivo especifico se coloca el nombre en lugar del punto)

**Maneras de crear y eliminar recursos**

- imperativa ==> kubectl create [recurso]

- declarativa ==> kubectl apply -f manifest-file.yaml

## what is a service?

A Service enables network access to a set of Pods in Kubernetes.
Es la manera en la que interactuamos con los pods dentro del cluster de kubernets.
En kubernets hay 4 tipos de servicios:

- **NodePort**(forma en que interactuamos con los puertos externos al cluter en el ambiente de desarrollo)
- **clusterIP**(permite la comunicacion entre pods que se encuentran dentro de un cluster)
- **LoadBalancer**(se usa en produccion,)
- **Ingress**(usado tanto en desarrollo como prodccion)

**NOTA** cada vez que un servicio es eliminado o destruido, cuando se reconstruye, este toma una ip diferente, lo cual es realizado por un servicio oculto llamado "DNS Service", el cual vive en el namespace llamado kube-system

## What is a namespace in Kubernetes?

Namespaces are a way to organize clusters into virtual sub-clusters — they can be helpful when different teams or projects share a Kubernetes cluster. Any number of namespaces are supported within a cluster, each logically separated from others but with the ability to communicate with each other.
**NOTA:**Cuando no definimos un namespace especifico, estaremos haciendo referencia al "default namespace", para hacer referencia a un namespace especifico se debe usar el flag -n [namespace], ejemplo:
**kubectl get all -n kube-system**

## Kubernets deployments

A Kubernetes Deployment is used to tell Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can scale the number of replica pods, enable rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary.

## Ingress Resource type

