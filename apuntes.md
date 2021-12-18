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

## Componentes de kubernets:

1. **Cluster**: Un clúster de Kubernetes es un conjunto de máquinas de nodos que ejecutan aplicaciones en contenedores. Si ejecuta Kubernetes, está ejecutando un clúster.

2. **Worker node (maquina virtual)**: al menos se debe tener uno, pero se pueden tener varios(worker node). se pueden ver como computadoras que ejecutan un contenedor.

3. **Pod**: son los que envuelven a los contedores para ser ejecutados. los pods son la unidad minima de kubernets.Un pod pods puede contener multiples contenedores, lo ideal es tener un contenedor en un pod.

cluster **>** nodos(worker nodes) **>** pods **>** contenedores(ideal solo 1)

4. **kubelete**: getiona los pods(contenedores) dentro del worker node y ademas se comunica con el master node

5. **Kube-proxy**: permite la comunicacion en red(network comunication) por fuera y dentro del worker node

6. **Master node**: it contains a bunch of differents things, we can have one or many master node, it allows to mantaining the worker nodes and the pods working inside the node.Contiene el API service que se comunica con el kubelete

**NOTA**: **kubectl** es el cli de k8s que nos permitira comunicarnos con el cluster(master node), permitiendonos indicarle que hacer, por ejemplo, le podemos indicar al master node que cree un recurso.

---

### COMANDOS DE KUBECTL

* kubectl get all