---
### encrypt db password
````
echo -n "Passwd123$" | base64
````
### create secret to store db password
````
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQxMjM=   # "password123"
````
---
````
apiVersion: apps/v1
kind: Deployment
metadata:
  name: student-app-backend
  labels:
    app: student-app-backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: student-app-backend
  template:
    metadata:
      labels:
        app: student-app-backend
    spec:
      containers:
        - name: student-app-backend
          image: your-dockerhub-username/student-app:latest
          ports:
            - containerPort: 8080
          env:
            - name: DB_URL
              value: jdbc:mariadb://mariadb:3306/student_db
            - name: DB_USERNAME
              value: admin
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secret
                  key: DB_PASSWORD
````
### service file

````
apiVersion: v1
kind: Service
metadata:
  name: student-app-service
spec:
  selector:
    app: student-app-backend
  ports:
    - port: 80
      targetPort: 8080
  type: LoadBalancer
````
