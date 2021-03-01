# Kubernetes cheatsheet

Kubernetes es un **orquestador de contenedores**:

- Administra el despliegue de contenedores
- Preparado para entornos de producción

Kubernetes dispone de herramientas para levantar contenedores:

- En alta disponibilidad
- De forma segura
- Aislados de otras cargas de trabajo
- Conectados entre sí
- Expuestos a Internet
- Conectados con servicios en la nube

## Links de introducción

- [¿Qué es Kubernetes? Web oficial](https://kubernetes.io/es/docs/concepts/overview/what-is-kubernetes/)
- [Cómic](https://cloud.google.com/kubernetes-engine/kubernetes-comic)
- [Guía ilustrada](https://www.cncf.io/wp-content/uploads/2020/08/The-Illustrated-Childrens-Guide-to-Kubernetes.pdf)

## Instala Kubernetes en local
- [Windows](https://docs.docker.com/docker-for-windows/#kubernetes)
- [Mac](https://docs.docker.com/docker-for-mac/#kubernetes)
- [Linux](https://microk8s.io/)


## Listas de recursos para Kubernetes
[Awesome Kubernetes](https://github.com/ramitsurana/awesome-kubernetes)

[Kubedex](https://kubedex.com/)

[DevOps Unlocked](https://devopsunlocked.com/kubernetes-curated-list-of-tools-and-resources/)


# Comandos básicos

Todas las interacciones con el clúster de Kubernetes se hacen a través del CLI `kubectl`

Crear (y actualizar) objetos a partir de archivos YAML: `kubectl apply -f <ARCHIVO>`

Listar objetos: `kubectl get <OBJETO>`. Por ejemplo: `kubectl get pod`

Ver todo el YAML de un objeto en concreto: `kubectl get <OBJETO> <NOMBRE> -o yaml`. Por ejemplo: `kubectl get pod nginx -o yaml`

Ver descripción y eventos de un objeto en concreto: `kubectl describe <OBJETO> <NOMBRE>`. Por ejemplo: `kubectl describe pod nginx`

Eliminar un objeto en concreto: `kubectl delete <OBJETO> <NOMBRE>`

## kubectl cheatsheets
[https://kubernetes.io/docs/reference/kubectl/cheatsheet/](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

[https://dzone.com/articles/kubectl-commands-cheat-sheet](https://dzone.com/articles/kubectl-commands-cheat-sheet)

Imprimible: [https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf](https://linuxacademy.com/site-content/uploads/2019/04/Kubernetes-Cheat-Sheet_07182019.pdf)

## Namespaces

Los namespaces se utilizan como una separación lógica entre distintos entornos o proyectos. Se puede desplegar el mismo YAML en distintos namespaces sin necesidad de cambiar el nombre de los objetos.

Para ejecutar cualquier comando en un namespace en concreto se utiliza el parámetro `-n <NAMESPACE>`

Si no se especifica, el comando se ejecutará en el namespace de nombre `default`


# Objetos de Kubernetes
## Nodos
Son los servidores físicos que forman parte del clúster.

Lista todos los nodos con `kubectl get nodes`

## Pod
Un Pod en Kubernetes es un objeto que define una **carga de trabajo**. Es decir, un Pod es uno o varios contenedores que son ejecutados conjuntamente en Kubernetes.
 
Crea un pod con `kubectl apply -f pod-nginx.yaml`

Lista todos los pod con `kubectl get pod`

Ver la descripción del pod:  `kubectl describe pod nginx`

Normalmente no creamos un pod en Kubernetes sino que creamos los siguientes objetos que a su vez crearan los pod necesarios para nuestra aplicación:

## Deployments
El objeto Deployment define una aplicación con varias réplicas, todas iguales. El Deployment a su vez crea y gestiona los pods que corresponden a esa definición.

Crea un deployment con `kubectl apply -f deployment.yaml`

Lista todos los deployment con `kubectl get deploy`

## DaemonSet
El objeto DaemonSet despliega un pod (solo uno) en cada nodo del clúster

Crea un DaemonSet con `kubectl apply -f daemonset.yaml`

Lista todos los DaemonSet con `kubectl get ds`

## Statefulset
El objeto Statefulset define una aplicación que tiene datos persistentes en disco (por ejemplo una base de datos). Es prácticamente igual que el deployment, pero levanta los pods siempre con un nombre fijo de forma que los datos pueden persistir entre despliegues y reinicios.

Crea un statefulset con `kubectl apply -f statefulset.yaml`

Lista todos los statefulset con `kubectl get statefulset`

## Job
El objeto Job define una carga de trabajo puntual, una tarea que finalizará con éxito en algún momento.

Crea un job con `kubectl apply -f job.yaml`

Lista todos los job con `kubectl get job`

## CronJob
El objeto CronJob crea jobs de forma periódica. Por ejemplo, todas las noches, para ejecutar tareas cada cierto tiempo

Crea un job con `kubectl apply -f cronjob.yaml`

Lista todos los job con `kubectl get cronjob`

## Service
Service representa un balanceador de carga entre varios pods que comparten una misma etiqueta, normalmente entre las réplicas de un mismo deployment o statefulset

Crea un service con `kubectl apply -f cronjob.yaml`

Lista todos los service con `kubectl get service`

## Ingress
### Ingress Controller
Un ingress controller sirve como punto de entrada a todo el tráfico HTTP del clúster. No viene incluido con Kubernetes, se puede desplegar fácilmente. El más básico y oficial es el [Nginx Ingress Controller](https://kubernetes.github.io/ingress-nginx/)

### Ingress
Ingress es un objeto que define unos parámetros HTTP que debe tener el tráfico de un servicio (Por ejemplo, un hostname concreto o un path concreto). Estos objetos configuran las rutas en el ingress controller.

Lista todos los ingress con `kubectl get ingress`

## ConfigMap
Configmap contiene una o varias variables que más adelante se pueden montar en cualquier carga de trabajo.


Crea un service con `kubectl apply -f configmap.yaml`

Lista todos los service con `kubectl get configmap`

Puedes crear un configMap a partir de un archivo o carpeta con `kubectl create configmap`


## Secret
Similar a configmap, pero los datos se almacenan codificados en base64

Crea un service con `kubectl apply -f secret.yaml`

Lista todos los service con `kubectl get secret`

Puedes crear un secret a partir de un archivo o carpeta con `kubectl create secret generic`

También puedes crear un secret especial con las credenciales para acceder a registries privados:

`kubectl create secret docker-registry`

## PersistentVolume
Un persistentVolume representa un disco persistente de cualquier tipo que está disponible para ser utilizado en Kubernetes.

Lo habitual no es crear estos persistentVolume de forma manual sino que una storageClass los cree automáticamente cuando detecta un nuevo persistentVolumeClaim

Crea un persistentVolume con `kubectl apply -f persistentvolume.yaml`

Lista todos los persistentVolume con `kubectl get pv`

Documentación sobre todos los sistemas compatibles: [https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes)

## PersistentVolumeClaim
Un persistentVolumeClaim representa una petición de disco persistente, con ciertos requisitos de tamaño y modos de acceso. Éstos son asociados con pod para habilitar persistencia de datos a sus contenedores. Kubernetes asociará un persistentVolumeClaim con un persistentVolume que cumpla sus requisitos.

Crea un persistentVolumeClaim con `kubectl apply -f persistentvolumeclaim.yaml`

Lista todos los persistentVolumeClaim con `kubectl get pvc`

## storageClass
Las storageClass se utilizan en conjunto con algún sistema que permita crear discos automáticamente. Su función es servir de enlace entre dicho sistema de discos (NFIS, iSCSI, AWS EBS, ...), para crear persistentVolumes que satisfagan los requisitos de todos los PersistentVolumeClaim. De esta forma, siempre que se requiera un disco, será creado automáticamente.

Crea una storageClass con `kubectl apply -f storageclass.yaml`

Lista todos los storageClass con `kubectl get sc`

Documentación sobre todos los sistemas compatibles: [https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner)

# Ejemplo Postgres

El archivo postgres.yaml contiene todos los objetos necesarios para desplegar un cluster de PostgreSQL en alta disponibilidad en Kubernetes (Services, StatefulSets y Secrets). 

Para levantarlo en cualquier Kubernetes sólo hay que ejecutar `kubectl apply -f postgres.yaml`

# Documentación oficial
Referencia de todos los objetos de Kubernetes: [https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.19/)

Arquitectura de Kubernetes: [https://kubernetes.io/docs/concepts/architecture/](https://kubernetes.io/docs/concepts/architecture/)

Todo sobre los pods: [https://kubernetes.io/docs/concepts/workloads/pods/](https://kubernetes.io/docs/concepts/workloads/pods/)


# Helm

<img src="../img/helm.svg" alt="Helm" width="200px">

Helm es un gestor de paquetes para Kubernetes. Permite instalar aplicaciones ya preparadas para desplegar en cualquier cluster de Kubernetes, administrarlar estas aplicaciones dentro del cluster (actualizarlas, personalizarlas, etc), y también puedes crear tus propias aplicaciones de forma que tus desarrollo también estén administrados en Helm.

Helm no es más que un CLI con el que puedes realizar todas estas acciones. Puedes instalarlo y comenzar a utilizarlo fácilmente en [https://helm.sh/docs/intro/quickstart/](https://helm.sh/docs/intro/quickstart/)

Al final Helm es simplemente un motor de templatizado para los YAML que ya conocemos de Kubernetes. Permite agrupar todos los YAML que componen una aplicación (Deployments, services, ingresses, configmaps, etc), y añadirles variables para que se puedan personalizar fácilmente. Una vez todos los YAML están listos, Helm permite subirlos a un repositorio para que puedan ser instalados por otros usuarios de Helm. Es un añadido muy sencillo para Kubernetes que le dota de una nueva funcionalidad. Es algo así como un `kubectl apply` con esteroides.

Las aplicaciones disponibles en Helm se llaman **charts**. Y las aplicaciones que tienes instaladas en tu cluster se llaman **releases**. 

Puedes encontrar todos los charts disponibles para ser instalados con Helm en [https://artifacthub.io/](https://artifacthub.io/). Es como DockerHub pero en lugar de imágenes de Docker para levantar en una máquina, encontrarás charts de Helm para levantar en un clúster de Kubernetes directamente, sin tener que crear tú los YAML. En casi todos los chart vienen instrucciones con los comandos necesarios para instalarlo, así como las variables para personalizarlo.

Puedes seguir practicando con Helm en esta guía: [https://helm.sh/docs/intro/using_helm/](https://helm.sh/docs/intro/using_helm/)


# Proyectos extra (day-2 operations)

Son proyectos del ecosistema de Kubernetes que prácticamente están en todos los clúster y facilitan tareas comunes:

[Prometheus](https://prometheus.io/) + [Grafana](https://grafana.com/): Solución de monitorización y alertas nativo

[FluentBit](https://fluentbit.io/): Solución de logging

[Velero](https://github.com/vmware-tanzu/velero): Solución para backups y disaster-recovery

[Cert-manager](https://cert-manager.io/docs/): Generación automática de certificados TLS para services e ingresses

[external-dns](https://github.com/kubernetes-sigs/external-dns): Integración con casi todos los proveedores DNS para creación automática de registros DNS que apunten al clúster, tanto para sevice como para ingress.

Todos ellos se pueden desplegar fácilmente con Helm.

Extra: Service Mesh

[Istio](https://istio.io/): Service Mesh para Kubernetes y máquinas virtuales


# UIs

[Kubernetic](https://www.kubernetic.com/)  Mi recomendación, es un cliente de escritorio con soporte para todas las acciones de kubectl y helm

[Dashboard oficial](https://github.com/kubernetes/dashboard) Es una app web que se despliega en el clúster. Tiene la ventaja de que al ser web está disponible en cualquier navegador con acceso al clúster.

He incluido el archivo `dashboard.yaml` para desplegar automáticamente el dashboard de Kubernetes con las opciones de autenticación y seguridad deshabilitadas.

[K9S](https://github.com/derailed/k9s) Si eres fan de usar la terminal, este cliente se ejecuta completamente en ella y ofrece una interfaz sencilla y rápida para acceder a todos los recursos del clúster.

