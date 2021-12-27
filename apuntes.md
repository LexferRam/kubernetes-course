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
- kubectl logs [name-pod] (debugg un pod) --follow=true **ver stream de logs**
- kubectl delete [resorce-name(deployment e.i)] [name-resorce] (eliminar un recurso) ejemplo: kubectl delete service mongo-srv
- kubectl delete all --all (elimina todo)
- mkubectl apply -f . ==> (si se coloca . se aplica para todo los archivos yaml, si se quiere un archivo especifico se coloca el nombre en lugar del punto)
- kubectl describe ingress (describe el ingress creado)
- kubectl get pv (obtiene los persistent volume)
- kubectl get pvc (obtiene los persistence volume claim)
- kubectl exec -it [nombre-pod] -- sh (explorar el contenedor de un pod)(usar ctrl+d para salir)

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
**mkubectl get all --all-namespaces**
**kubectl get ns**
**kubectl delete ns developers**

## Crear un namespace 

Para crear un namespace, crea un fichero YAML como el siguiente:

```yml
apiVersion: v1
kind: Namespace
metadata:
   name: developers
```

Para crear el namespace, ejecuta:

```cmd
 kubectl create -f ns-developers.yam
```

ver descripcion del namespace:

```cmd
kubectl describe ns developers
```


Los namespaces (espacios de nombres) en Kubernetes permiten establecer un nivel adicional de separación entre los contenedores que comparten los recursos de un clúster.

Esto es especialmente útil cuando diferentes grupos de DevOps usan el mismo clúster y existe el riesgo potencial de colisión de nombres de los pods, etc usados por los diferentes equipos.

## Kubernets deployments

A Kubernetes Deployment is used to tell Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can scale the number of replica pods, enable rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary.

## Ingress Resource type

Kubernetes Ingress is an API object that provides routing rules to manage external users' access to the services in a Kubernetes cluster, typically via HTTPS/HTTP. With Ingress, you can easily set up rules for routing traffic without creating a bunch of Load Balancers or exposing each service on the node.

**NGINX Ingress** Controller is a best-in-class traffic management solution for cloud‑native apps in Kubernetes and containerized environments.

Para habitar los distintos(segun su instalacion ) nginx ingress :

<https://kubernetes.github.io/ingress-nginx/deploy/#microk8s>

1. mkubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.30.0/deploy/static/mandatory.yaml
2. microk8s enable ingress

### Cómo configurar el DNS local usando el archivo/etc/hosts en Linux

DNS (Sistema o servicio de nombres de dominio) es un sistema/servicio de nombres descentralizado jerárquico que traduce los nombres de dominio a direcciones IP en Internet o una red privada y un servidor que proporciona dicho servicio se denomina servidor DNS.

**NOTA**: para ver info del host ejecutar: code /etc/hosts
proceso en windows:

- cmd como admin 
- moverse a C:\Windows\System32\Drivers\etc
- ejecutar: notepad hosts
- se debe ejecutar **kubectl describe ingress** para ver la ip que expone cada servicio y se debe modificar el archivo hosts colocando dicha IP seguido del dominio configurado en el ingress


### Obtener info de MicroK8s

```cmd
microk8s kubectl get nodes -o wide
```

### habilitar DNS

```cmd
microk8s enable dns
```

## Habilitar storage en MicroK8s para usar PV

```cmd
microk8s enable storage
```

## Eliminar pvc

```cmd
kubectl delete pvc <pvc-name>
```


# Resumen Curso Pelao Nerd

* When you deploy kubernetes, you get a cluster
* A  kubernetes cluster consistsof a set of worker machines, that runs containerized applications. Every cluster has at least one worker node 
* the worker node(s) host the pods that are the components of the application workload.
* the control plane manages the worker nodes and the pods in the cluster.A cluster usally runs multiple nodes, providingfault tolenrance and high availability.

**StatefulSet**
A Statefulset is a Kubernetes controller that is used to manage and maintain one or more Pods that uses volumes.(i.e DB pod)
Deployment is more suited to work with stateless applications.

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-csi-app-set
spec:
  selector:
    matchLabels:
      app: mypod
  serviceName: "my-frontend"
  replicas: 1
  template:
    metadata:
      labels:
        app: mypod
    spec:
      containers:
      - name: my-frontend
        image: busybox
        args:
        - sleep
        - infinity
        volumeMounts:
        - mountPath: "/data"
          name: csi-pvc
  volumeClaimTemplates:
  - metadata:
      name: csi-pvc
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 5Gi
      storageClassName: do-block-storage
```