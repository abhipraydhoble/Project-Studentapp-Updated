version: "3.8"
services: 
  backend:
    build:
      context: ./backend
      dockerfile: dockerfile
    ports:
      - 8080:8080
 #   depends_on:
  #     -db

  frontend:
    build: 
      context: ./frontend 
      dockerfile: dockerfile
    ports:
      - 80:80 
    depends_on:
       - backend

#volumes:
 #  mariadb_db:
