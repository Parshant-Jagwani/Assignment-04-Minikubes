# Assignment-04-Minikubes
## Files Included:
- **Images:** This folder contains image resources used in the portfolio website.
- **index.html:** The main HTML file that serves as the entry point for the portfolio site.
- **script.js:** JavaScript file responsible for adding interactivity and functionality to the site.
- **style.css:** Cascading Style Sheets file defining the visual appearance and layout of the website.
- **Dockerfile:** This file contains instructions for building a Docker image of the portfolio website. It specifies the environment and dependencies required for the website to run smoothly within a Docker container.
- **web-server-deployment.yaml:** Kubernetes deployment file for deploying the web server component of the portfolio website.
- **web-server-ingress.yaml:** Kubernetes Ingress file for configuring external access to the web server.
- **web-server-service.yaml:** Kubernetes Service file for exposing the web server to other components within the cluster.

# Minikube Installation:

## a. Use a virtualization platform (e.g., VirtualBox) if not already installed. You can use your host OS if you have Docker Desktop Installed.
   1. Verify docker setup:
      - Commands:
        ```
        docker version
        docker info
        docker start
        ```

## b. Install Minikube by following the official installation instructions for your operating system.
   1. Download Minikube from [here](https://minikube.sigs.k8s.io/docs/start/).

## c. Verify the installation by running basic Minikube commands and checking the version.
   - Commands:
     ```
     minikube version
     minikube start
     minikube dashboard
     ```
  - Output:
     ```
     PS C:\Users\parsh> minikube version
    W0216 13:13:05.896253   12060 main.go:291] Unable to resolve the    current Docker CLI context "default": context "default": context   not found: open C:\Users\parsh\.  docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba1   41d5f4133a33f0688f\meta.json: The system cannot find the path  specified.
    minikube version: v1.32.0
    commit: 8220a6eb95f0a4d75f7f2d7b14cef975f050512d
    PS C:\Users\parsh> minikube start
    W0216 13:13:11.963640   13424 main.go:291] Unable to resolve the    current Docker CLI context "default": context "default": context   not found: open C:\Users\parsh\.  docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba1   41d5f4133a33f0688f\meta.json: The system cannot find the path  specified.
    😄  minikube v1.32.0 on Microsoft Windows 11 Home 10.0.22621.3155   Build 22621.3155
    ✨  Using the docker driver based on existing profile
    👍  Starting control plane node minikube in cluster minikube
    🚜  Pulling base image ...
    🔄  Restarting existing docker container for "minikube" ...
    🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
    🔗  Configuring bridge CNI (Container Networking Interface) ...
    🔎  Verifying Kubernetes components...
        ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
        ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
        ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
    💡  Some dashboard features require the metrics-server addon. To    enable all features please run:
    
            minikube addons enable metrics-server
    
    🌟  Enabled addons: storage-provisioner, default-storageclass,  dashboard
    🏄  Done! kubectl is now configured to use "minikube" cluster and   "default" namespace by default
    PS C:\Users\parsh> minikube dashboard
    W0216 13:14:41.533595   20892 main.go:291] Unable to resolve the    current Docker CLI context "default": context "default": context   not found: open C:\Users\parsh\.  docker\contexts\meta\37a8eec1ce19687d132fe29051dca629d164e2c4958ba1   41d5f4133a33f0688f\meta.json: The system cannot find the path  specified.
    🤔  Verifying dashboard health ...
    🚀  Launching proxy ...
    🤔  Verifying proxy health ...
    🎉  Opening http://127.0.0.1:55830/api/v1/namespaces/   kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in     your default browser...
     ```

# Deploying Applications:

## a. Create a custom Docker image for the application, which displays the pod name. (Use any repo from GitHub having a Docker File)
   - For custom Docker Image, I have used my portfolio website.
   - Files Included:
     - Images: This folder contains image resources used in the portfolio website.
     - index.html: The main HTML file that serves as the entry point for the portfolio site.
     - script.js: JavaScript file responsible for adding interactivity and functionality to the site.
     - style.css: Cascading Style Sheets file defining the visual appearance and layout of the website.
     - Dockerfile: This file contains instructions for building a Docker image of the portfolio website. It specifies the environment and dependencies required for the website to run smoothly within a Docker container.
   - **building Docker Image:** ```docker build -t web-server-image . ```
   - **Pushing Docker Image to Docker Hub:**
      *before that create docker project on doker hub project-name:pj-portfolio*
      ```
      docker login
      docker tag web-server-image parshantjagwani/pj-portfolio:web-server-image
      docker push parshantjagwani/pj-portfolio:web-server-image
      ```
   - **Output:**
      ```
      The push refers to repository [docker.io/parshantjagwani/pj-portfolio]
      0bc3febd1338: Pushed
      009507b85609: Mounted from library/nginx
      fbcc9bc44d3e: Mounted from library/nginx
      b4ad47845036: Mounted from library/nginx
      eddcd06e5ef9: Mounted from library/nginx
      b61d4b2cd2da: Mounted from library/nginx
      b6c2a8d6f0ac: Mounted from library/nginx
      571ade696b26: Mounted from library/postgres
      web-server-image: digest: sha256:e90d38dbd5d6e2950c8724e3fb880331b2829d090b2b1c1610615cd6d5c2ef9f size: 1989
      ```


## b. Create three deployments using the custom Docker image.
   - For creating 3 deployments, I have mentioned 'replicas:4' for 4 pods and one pod is accessed for testing purposes.
     - web-server-deployment.yaml
   - **Command:**
      ```
      kubectl apply -f web-server-service.yaml
      ```
  - **Output:**
      
      ```
        NAME                      TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
        kubernetes                ClusterIP      10.96.0.1        <none>        443/TCP        6d2h
        web-server-loadbalancer   LoadBalancer   10.103.14.210    <pending>     80:31284/TCP   2m4s
        web-server-nodeport       NodePort       10.110.123.110   <none>        80:30001/TCP   2m4s
        web-server-service        ClusterIP      10.106.12.91     <none>        80/TCP         2m6s
      
      ```
      

## c. Verify the successful deployment of the pods.
   - Command:
     ```
     kubectl get deployments
     ```

# Setting Up Services:

## For Setting Up Services: I had Create web-server-service.yaml in which all the services are given below named as:
   - web-server-nodeport: as NodePort
   - web-server-service: as ClusterIP
   - web-server-loadbalancer: as LoadBalancer
   **Command:**: 
      `kubectl apply -f web-server-service.yaml`

   **Output:**:

      ```
      PS C:\Users\parsh\OneDrive\Desktop\CODE-REPO\PJ-Portfolio\Parshant-Jagwani-Portfolio> kubectl get services
      NAME                      TYPE           CLUSTER-IP            EXTERNAL-IP   PORT(S)        AGE
      kubernetes                ClusterIP      10.96.0.1             <none>        443/TCP        6d2h
      web-server-loadbalancer   LoadBalancer   10.103.14.210         <pending>     80:31284/TCP   2m4s
      web-server-nodeport       NodePort       10.110.123.110        <none>        80:30001/TCP   2m4s
      web-server-service        ClusterIP      10.106.12.91          <none>        80/TCP         2m6s
    ``` 
      

## a. Create a NodePort service to expose one of the deployments.
   - web-server-nodeport: as NodePort

## b. Create a ClusterIP service to expose the second deployment.
   - web-server-service: as ClusterIP

## c. Create a LoadBalancer service to expose the third deployment.
   - web-server-loadbalancer: as LoadBalancer

## d. Verify the successful creation of the services.
   - Command:
     ```
     kubectl get services
     ```

# Accessibility Demonstration:

## a. Explain why pods are inaccessible outside the cluster when using the ClusterIP service.
   - Due to External-IP not being assigned, I accessed it using the following command:
     - Command: `kubectl port-forward service/web-server-service 8080:80`

## b. Demonstrate how pods are accessible within the cluster using the NodePort service.
   - Unable to access, maybe I didn't mention the IP completely in '- name: http/192.168.49.2' in 'web-server-nodeport'.

## c. Demonstrate how pods are accessible from outside the cluster using the LoadBalancer service.
   - This pod is inaccessible outside the cluster due to EXTERNAL-IP being Pending.
   - To achieve accessibility outside the cluster, we have to create Minikube tunnel.
     - Command: `minikube tunnel`


