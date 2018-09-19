# Prerequisites

**Install a hypervisor**
Follow these instructions. (When in doubt, Virtualbox)

**Installing kubectl from Terminal**
Follow these instructions.

**Installing Minikube from Terminal**
**OSX:** curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.2/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
**Linux:** curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.28.2/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
**Windows:** Download here

# Part 1 - Lower level objects

This section covers pods and services. Pods are a lower level object of the Kubernetes workload API. For more information [go here](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/).

Start minikube 

`minikube start --verbose`

Check to make sure the minikube context is active

`kubectl config get-contexts`

Take a note of all the pods running by default

`kubectl get pods --all-namespaces`

Apply 01-low-level-objects

`kubectl apply -f 01-low-level-objects`

Check out pod and service details

`kubectl describe pod/nginx`

`kubectl describe svc/nginx`

Although services "work" they have to be port-forwarded to because really these are supposed to be _actual_ loadbalancers.

Let's port-forward to the nginx service:

`kubectl port-forward svc/nginx 8080:80`

You can now view your app at `localhost:8080`

Pods are very basic, non-lifecycle controlled types. Let's see what that means! In the real world a pod cannot survive a node restart, however, in minikube-land it can. We can simulate a terminated pod though!

`kubectl get pods`

`kubectl delete pods/nginx-pod`

`kubectl get pods`

The pod is gone!

Do a kubectl apply to bring it back

`kubectl apply -f 01-low-level-objects`

`kubectl get pods`

Notice how the service knew that it was unchanged?

**Homework**
* How did the service know that it was unchanged?
* How are we dictating order of evaluation? Is there another way? What happens if you reverse them?
* How does the service identify what pod to expose?
* Is there another way of exposing our nginx pod?
* Are non-yaml files evaluated when using `kubectl apply -f`?

# Part 2 - Higher level objects

Deployments, DaemonSets, ReplicaSets, and StatefulSets. For more information on each higher level workload API object and their respective controllers [visit here](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/).

Let's start with a fresh page.

`kubectl delete -f 01-low-level-objects`

`kubectl apply -f 02-higher-level-objects && kubectl get pods -w`

Once wordpress is a `Running` state...

`kubectl port-forward svc/wordpress 8080:80`

We're going to step through this one together...