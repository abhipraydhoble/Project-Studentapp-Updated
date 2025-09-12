---
## deployment.yaml
````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: student-frontend
  labels:
    app: student-frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: student-frontend
  template:
    metadata:
      labels:
        app: student-frontend
    spec:
      containers:
        - name: student-frontend
          image: your-dockerhub-username/student-frontend:latest
          ports:
            - containerPort: 80
````
---
### service.yaml
````
apiVersion: v1
kind: Service
metadata:
  name: student-frontend-service
spec:
  selector:
    app: student-frontend
  ports:
    - port: 80
      targetPort: 80
  type: LoadBalancer   
````
