# Kubernetes Application Deployments and Rollout and scalling

$ kubectl replace -f new-nginx.yaml --record# cloudapplication-kubernetes
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

It should be noted that the deployments manages the ReplicaSets and the ReplicaSets (rs) manages the pods, hence:

$ kubectl get rs

gives a short summary of the ReplicaSet deployed.

to isolate your pods created using labels used in creating the deployment for example "http-server"

$ kubectl get pod -l "service in (http-server)"

UPDATING YOUR ROLLOUT/DEPLOYMENT

This is where the fun begins:

we can update the deployment from first the YAML file:


# First 
we update the number of replicas from the yaml from 3 file to 5. Next we edit the spec:

maxSurge:

This is the number of pods that can be created with auto-configuration of pods to deal with the surge of request on the application. This can be a whole number or a percentage. e.g. if the replicas where 5 and a maxSurge was 2. this means the total number of pods tha can be deployed at any given situation is 5. 

minReadySeconds:
Since kubernetes engine assume your application is available once the pods are deployed by default, the service maynot be available until all pods are deployed if you configure your setup leaving this field empty. hence the minReadySeconds will be the bootup time for your application service to be available, use your discretion. 

maxUnavailable:
this is the amount of pods that can be deleted when the demand on the application reduces. this field cannot be zero, so the maxUnavailable is set to 1.

save the file as new-nginx.yaml
roll this update using "replace"


$ kubectl replace -f new-nginx.yaml --record

in this repo its version2.


# The second way 
To roll out update we use "set image":
This current image is 1.10.2 from the yaml file. we can change this image using the set image as in this format:

# format

$ kubectl set image deployment <deployment> <container>=<image> --record
  
  
# example

$ kubectl set image deployment nginx nginx=nginx:1.12.3 --record

this gives: deployment "nginx" image updated


# Lastly:
we can edit a deployment. we make the changes on the yaml file and edit the deployment simply using edit in our case this is version3 shown below: This will take u to a vim edit page: 


$ kubectl edit deployment nginx --record

once exiting the vim editor the update will be noted.
 

this gives us version 3 or our deployemnt.
 
 for us to see all deployments:
 
 $ kubectl rollout status deployment nginx 
 
this takes time.


you can "pause" deployment rollout, "resume" deployment rollout
 
but to see the current status:

use $ kubectl rollout status deployment/nginx

to see a history of all the rollout: use 

$ kubectl rollout history deployment/nginx
 
 this gives:
 
deployments "nginx"

REVISION  CHANGE-CAUSE

1         kubectl replace --filename=new-nginx.yaml --record=true

2         kubectl set image deployment nginx nginx=nginx:1.12.3 --record=true

3         kubectl edit deployment nginx --record=true


you can also use describe deployment as used above to see each of the steps taking so far:

# ROLLING Back

if a rollout gets stuck, you can troubleshoot by first seeing details in history of a revision using the following

$  kubectl rollout history deployment/nginx --revision=1

This is where the record comes in handy. you can even rollback to the first deployment should in case the rollout gets stuck and rollout again making edition as mentioned above.

you can scale deployment by simply using: 


$ kubectl scale deployment nginx-deployment --replicas=12

this scales the deployment. one can even deploy auto scaling as you would in EC2 using the following

$ kubectl autoscale deployment nginx-deployment --min=3 --max=15

This sets the current deployment of pods to be a minimum of 3 and a maximum of 15.








reference: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/



