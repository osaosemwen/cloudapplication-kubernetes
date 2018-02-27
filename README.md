# cloudapplication-kubernetes
Before showing rolling out, updating application creating replicas etc on clusters, It is pertinent to read the introduction in the previous page README cloudapplication.
Next, a Cluster is a set or a group of instances, or compute engines connected to function as one unit. 
A pod, could be any type of container, it could be a container that embedds secrets or functions which can be hidden or seen in the instance, it could also be what holds a 
deployment of an application.
Now the Instance can be a monolith or a simple instance which has the basics CPU, storage size, labels.
Basically, most pods have the following: apiVersion, Kind, labels alias name, specs. it is essential to include this info in your yaml file.

now lets first work with a simple application, before testing a more complex application: in this tutorial, we would create a deployment, update it, make a rollback, scale it up, scale it down ..... and then delete the deployment. 
CREATING A Deployment
create the deployment by running the file:
kubectl
