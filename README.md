# CKAD

CKAD prepration Collection

## Start of the Journey

No idea about the exam. As per suggestion from experiance folks took the [Udemy course](https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/12299352#overview).

### Course details lecture by lecture

- Section 1:
  - Lecture 1 Introduction to the course.
  - Lecture 2 Different courses of Kubernetes on Udemy by Instructor.
  - Lecture 3 Link to other courses.
  - Lecture 4 Details about the certification
    - Online exam.
    - Can refer K8s documentation.
    - [Certified Kubernetes Application Developer](https://www.cncf.io/certification/ckad/)
    - [Candidate Handbook](https://www.cncf.io/certification/candidate-handbook)
    - [Exam Tips](https://docs.linuxfoundation.org/tc-docs/certification/tips-cka-and-ckad)
    - `DEVOPS15` code for 15% discount
- Section 2:
  - Lecture 1 All the aspects of kubernetes architecture like apiserver, controller, scheduler, kubelet, container runtime, etcd, worker node, and master node. Let's see a small gist about each of them. 
    - apiserver - This is the one to which the schedular and controller send the signal and API-server passes those signals to the kubelet.
    - controller - This is the brain for the nodes, pods and other services that take care of any resource that goes down.
    - schedular - This program will decide to which node the pod will get create as per user request.
    - kubelet - This is the point of contact on the worker node.
    - container runtime - This is the platform where the container will run in our case we are using docker.
    - etcd - This is the database to store data in the form of key-value pairs.
    - worker node - The worker node consists of kublet and container runtime to deploy containers in the form of pods.
    - Master node - This consists of api-server, scheduler, and controller.
  - Lecture 2 Docker vs Containerd
    - To understand this first we make one thing clear that docker is not exactly a runtime environment it is a tool that consists of runtime environment, volume etc.
    - Kubernetes has an interface called CRI(Container runtime interface) using which different runtimes like rkt, docker can orchestrate by kubernetes but the condition is that they should follow OCI(open container initiate) standard but Docker did not support that for a temp patch k8s introduce dockershim but later a daemon called containerd pulled out from docker and now k8s is directly interacting with containerd for which we have a cli like ctr, nerdctl etc. Containerd maintains the lifecycle of a container.
  - Lecture 3
    - Introduction to the pod which is an object in Kubernetes inside which container is running.
    - Can we run multiple containers inside one pod? Yes, we can but running the same kind of container in the same pod is not the right practice though we can run a helper container in the same pod.
    - Containers in the same pod share the same network and volumes by default.
    - `kubectl run nginx --image nginx`: Command to run a pod with a particular image.
    - `kubectl get pods`: Command to list down all the pods.
  - Lecture 4
    - Introduction to the definition file - A file has 4 sections `apiVersion`, `kind`, `metadata`, and `spec`
    - apiVersion: Different objects have different versions
      - Pod: v1
      - Service: v1
      - ReplicaSet: app/v1
      - Deployment: apps/v1
    - kind: here we will mention the object that we have to create.
    - metadata: This section includes the details about the object. The parents should have less space than the child and the siblings should be on the same line.
    - spec: Contains information related to the container.
    - `kubectl create -f definition.yaml`
    - `kubectl get pods`
    - `kubectl get pods -o wide`
    - `kubectl describe pod podName`
    - create a pod with image name `kubectl run podName --image=imageName`
    - To create a yaml file `kubectl run podName --image=imageName --dry-run=client -o yaml > filename.yaml`
    - Details about pod `kubectl describe pod podName -n namespace`
    - `kubectl edit pod podName`
    - `kubectl delete pod podName`
  - ReplicaSet
    - This is a controller which maintains the mentioned number of replicas of the Pod.
    - Load balancing and availability are been taken care with the replicaset.
    - Replicaset can create pods in other nodes as well.
    - ReplicationController Definition file
      ```
        apiVersion: v1
        kind: ReplicationController
        metadata:
          name:
          labels:
        spec:
          template:
            metadata:
              name:
              labels:
            spec:
              containers:
              - name:
                image:
          replicas:
      ```
    - Replicaset definition file
      ```
        apiVersion: apps/v1
        kind: ReplicaSet
        metadata:
          name:
          labels:
        spec:
          template:
            metadata:
              name:
              labels:
            spec:
              containers:
              - name:
                image:
          replicas:
          selector:
            matchLabels:
              type: front-end
      ```
    - Commands to scale the replicas
      - update the definition file and run `kubectl replace -f replicaset-definition.yaml`
      - `kubectl scale --replicas=6 -f replicaset-definition.yaml`
      - `kubectl scale --replicas=6 replicaset my-app-replicaset`
      - `kubectl delete replicaset replicasetName`
