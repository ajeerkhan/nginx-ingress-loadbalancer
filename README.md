# nginx-ingress-loadbalancer
Dmonstrate how to setup application load balancer that would handle the traffic to backend services. It is created using ingress solution avaialble for k8s, and deployed in GCP.

This document explain the steps to be performed to setup ingress in k8s cluster using nginx load balancing solution in Google cloud platform.

# Prerequisite:
    1. GCP account
    2. Kubernetes cluster setup in GCP.

# The steps for setting up Nginx ingress controller. 
    Step #1. Setup Ingress controller
    Step #2. Expose Ingress controller as service


# Step #1. Setup Ingress controller: 
    Run below kubectl command to create ingress controller from "ingress-controller-setup.yaml" file.
        kubectl create -f ingress-controller-setup.yaml

    Run below command to verify the necessary PODs are created and running for ingress controller.
        kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx --watch

# Step #2. Expose Ingress controller as service: 
    Run below kubectl command to create and expose service k8s object from "ingress-service-setup" file.
        kubectl create -f ingress-service-setup.yaml

Thats all!!! you are good to go ahead to define the routes for your backend services / application.



# Load Balancing a demo application: 
Echo-color is simple application that echo the "color" of application. Application is built using node / express server. It uses the prebuilt image "ajeerkhan/k8s:echo-color". 

Note: you can update the image according to your application needs.

Run below to setup two services such as green and blue. Service green echo color as "Green", service blue echo its color as "Blue".

    kubectl create -f echo-color-demo-app/

Above command create deployment and service object for service green &  blue.
Note: It is recommended to expose the backend services as ClusterIP to stop exposing it outside directly. However sample applications are exposed as "NodePort" just for testing purposes.


Right, its time to define ingress rule to manage the traffic to background services.

Run below to create ingress k8s object. It defines two routes based on the path /green, and /blue. 

    kubectl create -f backend-rules/

/blue -> if you access http://ingress-service-ip-or-domain-mapped/blue, you will get "Blue" message.
/green -> if you access http://ingress-service-ip-or-domain-mapped/green, you will get "Green" message.

Like above, you are allowed to create 'N' ingress objects defining various routes....



