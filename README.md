# cloudapplication-kubernetes
Before showing rolling out, updating application creating replicas etc on clusters, It is pertinent to read the introduction in the previous page README cloudapplication.
Next, a Cluster is a set or a group of instances, or compute engines connected to function as one unit. 
A pod, could be any type of container, it could be a container that embedds secrets or functions which can be hidden or seen in the instance, it could also be what holds a 
deployment of an application.
Now the Instance can be a monolith or a simple instance which has the basics CPU, storage size, labels.
Basically, most pods have the following: apiVersion, Kind, labels alias name, specs. it is essential to include this info in your yaml file.

now lets first work with a simple application, before testing a more complex application: in this tutorial, we would create a deployment, update it, make a rollback, scale it up, scale it down ..... and then delete the deployment. 



CREATING A Deployment

first create a project, then deploy your cluster on either minikube or on google cloud kubernetes engine. after giving your cluster a name. In my case google cloud engine: connect to it using 

$ gcloud container clusters get-credentials cluster name --zone us-east1-a --project project-name

create the deployment by running the file:


$ kubectl create -f cloudapplication/nginx.yaml --record

deployment "nginx" created


The --record makes it possible to track and monitor changes on the pods. you can use create or apply to deploy the yaml file.

$ kubectl get deployments 

This shows the name & state, and other information of the deployment "nginx"

Check the desciption (full detail- this will be a useful line for troubleshooting and viewing details)of the deployment:

$ kubectl describe deployments


to see the deployment rollout use (although this is also one of the lines from the describe deployments) :

$ kubectl rollout status deployment/nginx

It should be noted that the deployments manages the replica sets and the replicasets (rs) manages the pods, hence:

$ kubectl get rs
gives
