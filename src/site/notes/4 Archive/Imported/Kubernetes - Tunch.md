---
{"dg-publish":true,"permalink":"/4-archive/imported/kubernetes-tunch/"}
---

# Meta
[Lab900](Lab900.md)
[Tunch](Tunch.md)
[[3 Resources/Programming\|Programming]]
[[Kubernetes\|Kubernetes]]

# Presentation


## Why


### Lab900 Values     :ATTACH:

![img](/Users/marcel/org/.attach/39/9f8823-badd-4d66-a30a-fa8a99ce1890/_20220408_101413lab900-values.png)


### Lab900 Coding Values & Ambitions

We don’t have this written down but it would probably be…

1.  Developer Productivity

2.  Resilience

    -   **SRE:** Site Reliability Engineering

3.  Scalability

4.  Replicability

    Creating new environments should be easy.
    Or even do it locally?


### Reuse skills across cloud providers

We want to use Google Cloud but yeah…


### Docker is great     :ATTACH:

![img](/Users/marcel/org/.attach/39/9f8823-badd-4d66-a30a-fa8a99ce1890/_20220408_101208docker-meme.jpg)


### Need a solution for multiple Docker microservices


### [The 12 Factor App](https://12factor.net/)

-   **Codebase:** One codebase tracked in revision control, many deploys
-   **Dependencies:** Explicitly declare and isolate dependencies
-   **Config:** Store config in the environment
-   **Backing services:** Treat backing services as attached resources
-   **Build, release, run:** Strictly separate build and run stages
-   **Processes:** Execute the app as one or more stateless processes
-   **Port binding:** Export services via port binding
-   **Concurrency:** Scale out via the process model
-   **Disposability:** Maximize robustness with fast startup and graceful shutdown
-   **Dev/prod parity:** Keep development, staging, and production as similar as possible
-   **Logs:** Treat logs as event streams
-   **Admin processes:** Run admin/management tasks as one-off processes


## How


### Intro to Kubernetes objects

1.  Nodes     :ATTACH:

    Nodes are the machines that collectively form the cluster.
    
    ![img](/Users/marcel/org/.attach/39/9f8823-badd-4d66-a30a-fa8a99ce1890/_20220408_101909module_01_cluster.svg)

2.  Pods

    A Pod is a container (or multiple containers) running as a whole (on a Node).

3.  Deployments     :ATTACH:

    A Deployment manages creating Pods, scaling, health checking and updating.
    
    ![img](/Users/marcel/org/.attach/39/9f8823-badd-4d66-a30a-fa8a99ce1890/_20220408_101923module_02_first_app.svg)

4.  Services     :ATTACH:

    Services group together the pods in a Deployment and expose the whole group as a single endpoint (IP) within the cluster.
    
    How do you access other services?
    
    -   The cluster creates DNS bindings for services
    -   IP address should work too
    
    ![img](/Users/marcel/org/.attach/39/9f8823-badd-4d66-a30a-fa8a99ce1890/_20220408_101933module_05_scaling2.svg)

5.  Custom Resources

    The K8s resource model is extensible with custom resources, so you can create other things like pods, deployments, etc.


### Basic k8s operations

1.  Creating a deployment

        # myapp-deployment.yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: myapp-api
          labels:
            app.kubernetes.io/name: myapp
            app.kubernetes.io/component: api
        spec:
          selector:
            matchLabels:
              app.kubernetes.io/name: myapp
              app.kubernetes.io/component: api
          replicas: 2
          template:
            metadata:
              labels:
                app.kubernetes.io/name: myapp
                app.kubernetes.io/component: api
            spec:
              containers:
              - name: api
                image: nginx
    
        kubectl apply -f myapp-deployment.yaml
        kubectl deployment list
        kubectl pod list

2.  Creating a service

        # myapp-service.yaml
        apiVersion: v1
        kind: Service
        metadata:
          name: myapp-api
          labels:
            app.kubernetes.io/name: myapp
            app.kubernetes.io/component: api
        spec:
          selector:
            app.kubernetes.io/name: myapp
            app.kubernetes.io/component: api
          ports:
          - protocol: TCP
            port: 80
            targetPort: 80
    
        kubectl apply -f myapp-service.yaml
        kubectl get service

3.  Writing great deployments

    1.  Update without downtime
    
        Use `maxSurge` to let it scale up when it’s updating pods one by one.
        
            apiVersion: apps/v1
            kind: Deployment
            # ...
            spec:
              strategy:
                type: RollingUpdate
                rollingUpdate:
                  maxSurge: 1
                  maxUnavailable: 0
    
    2.  Keep resource usage in check
    
        This is especially important when you have multiple environments on the same cluster.
        
            apiVersion: apps/v1
            kind: Deployment
            # ...
            
            spec:
              template:
                spec:
                  containers:
                    - name: myapp
                      resources:
                        requests:
                          cpu: 100m
                          memory: 128Mi
                        limits:
                          cpu: 2
                          memory: 2G
    
    3.  Configure deployments
    
        As per [Factor III](https://12factor.net/config), store configuration in environment variables. This is the most universally supported way to inject variables during the runtime of a container. 12 Factor requires strict separation of config and code.
        
        Spring Boot etc can understand your application properties through environment variables automatically.
        
            apiVersion: apps/v1
            kind: Deployment
            # ...
            spec:
              template:
                spec:
                  containers:
                  - name: myapp
                    env:
                    - name: JAVA_OPTS
                      value: "-Xms{{ .Values.heapSize }} -Xmx{{ .Values.heapSize }}"
                    - name: MYAPP_API_USERNAME
                      valueFrom:
                        secretKeyRef:
                          name: myapp-api-secret
                          key: username
                    - name: MYAPP_API_PASSWORD
                      valueFrom:
                        secretKeyRef:
                          name: myapp-api-secret
                          key: password
        
        There is straight *config* and there are also *secrets*. You store them separately (so an admin can manage them) and give permissions to the pods.
        
        Config/secrets can be attached as either:
        
        -   Environment variables
        -   Files through a volume mount
        
        This is the way you can integrate with the other services of your cloud provider. Azure has a Key Vault Provider:
        
            apiVersion: apps/v1
            kind: Deployment
            # ...
            spec:
              template:
                spec:
                  containers:
                  - name: myapp
                    volumeMounts:
                    - name: secrets-store01-inline
                      mountPath: "/mnt/secrets-store"
                      readOnly: true
                    env:
                    - name: SECRET_USERNAME
                      valueFrom:
                        secretKeyRef:
                          name: foosecret
                          key: username
                  volumes:
                  - name: secrets-store01-inline
                    csi:
                      driver: secrets-store.csi.k8s.io
                      readOnly: true
                      volumeAttributes:
                        secretProviderClass: "azure-sync"


### Completing the Pipeline

1.  Helm

    K8s already makes deploying a lot easier, but when the infrastructure of your app changes, it can be annoying to update. Or just updating the deployment every time with a new image tag can become cumbersome.
    
    Enter Helm, which packages a bunch of K8s manifests into a *Chart*, that’s versioned and gets deployed in one go.

2.  Skaffold

    Has a great dev workflow:
    
    -   Tracks file changes
    -   Rebuilds your containers
    -   Instantly redeploys them
    
    You set up Skaffold to connect to Helm so that it:
    
    -   Builds containers
    -   Injects tags into your Helm chart
    -   Deploys a new version of the chart


### Advanced Operations

1.  Observability

    Microservices need more complex tracing to be able to know what’s going out throughout the entire application.

2.  Service Fabric: Istio


### Managed Kubernetes

1.  Cloud Run

2.  Autoscaling

