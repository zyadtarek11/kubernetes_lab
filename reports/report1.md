# Kubernetes Resource Requests and Limits

In Kubernetes, resource requests and limits are mechanisms to control how much CPU and memory a container can use.

## Resource Requests

A resource request is the amount of CPU or memory that Kubernetes guarantees to a container that When you send a request, Kubernetes ensures that the container gets at least the requested amount of resources. 

## Resource Limits

A resource limit is the maximum amount of CPU or memory that a container is allowed to use. If a container tries to use more than the specified limit, Kubernetes will throttle the container's resource usage or, in the case of memory, terminate the container to prevent the container from consuming all the resources on a node, ensuring fair resource distribution among containers.
