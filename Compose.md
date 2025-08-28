# Docker Compose
- Docker Compose is a tool for defining and running multi-container applications.
- It is the key to unlocking a streamlined and efficient development and deployment experience.
- Compose simplifies the control of your entire application stack, making it easy to manage services, networks, and volumes in a single YAML configuration file.
- Then, with a single command, you create and start all the services from your configuration file.


---
# docker-compose installation

````
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d'"' -f4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
````
````
sudo chmod +x /usr/local/bin/docker-compose
````
````
docker-compose --version
````
````
docker-compose build
````
````
docker-compose up -d
````
````
docker-compose down
````
