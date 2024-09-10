# kubernetes_lab on minikube 😄
Q1 & Q2) How many pods and nodes exist on the system?
![There are no pods created but there is a node created already named minikube](<Screenshot from 2024-09-10 12-43-19.png>)

Q3) Create a new pod with the nginx image.
![Creating an nginx image](<Screenshot from 2024-09-10 13-52-20.png>)

Q4) Which nodes are these pods placed on?
![alt text](<Screenshot from 2024-09-10 12-48-26.png>)

Q5) Create pod from the below yaml using kubectl apply command.

apiVersion: v1
kind: Pod
metadata:
  name: webapp
  namespace: default
spec:
  containers:
  - image: nginx
    imagePullPolicy: Always
    name: nginx
  - image: agentx
    imagePullPolicy: Always
    name: agentx
![alt text](<Screenshot from 2024-09-10 13-04-21.png>)

Q6 & 7 & 8) How many containers are part of the pod webapp and What images are used in the new webapp pod and What is the state of the container agentx in the pod webapp?
1 container created
![alt text](<Screenshot from 2024-09-10 13-10-17.png>)

Q9) Why do you think the container agentx in pod webapp is in error?
because there is no image named agentx
Q10) Delete the webapp Pod.
![alt text](<Screenshot from 2024-09-10 13-14-07.png>)
Q11) Create a new pod with the name redis and with the image redis123.
•	Name: redis
•	Image Name: redis123
![alt text](<Screenshot from 2024-09-10 13-16-49.png>)
Q12) Now change the image on this pod to redis.
Once done, the pod should be in a running state
![alt text](<Screenshot from 2024-09-10 13-32-58.png>)
Q13 & 14) Create a pod called my-pod of image nginx:alpine and Delete the pod called my-pod.
![alt text](<Screenshot from 2024-09-10 13-35-15.png>)

Thanks for reading till here 🥰


