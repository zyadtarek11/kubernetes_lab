Q1) What is a namespace in Kubernetes, and why is it used?
namespace is a way to provide a mechanism for isolating groups of resources within the same cluster, It is applicable only for namespaced objects (e.g. Deployments, Services, etc.) and it's names need to be unique,
It is used to divide cluster resources between multiple users 

Q2) How do you create a new namespace in Kubernetes using the kubectl command?
by entering this command ```kubectl config set-context --current --namespace=<insert-namespace-name-here>```

Q3) How can you list all namespaces in a Kubernetes cluster?
