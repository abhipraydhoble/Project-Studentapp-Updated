# Step 1: Use the official Node.js image as a parent image
FROM node:16-slim

# Step 2: Set the working directory inside the container
WORKDIR /app

# Step 3: Copy the frontend project files into the container
COPY . /app

# Step 4: Install the frontend dependencies
RUN npm install

# Step 5: Set the environment variable for the backend API URL
# Replace the placeholder with your actual backend server IP
#ENV REACT_APP_BACKEND_URL=http://172.17.0.3:8080   
# manual edit file vim src/api/userService.js and add your public backend server IP

# Step 6: Build the frontend for production
RUN npm run build

# Step 7: Install Apache2 for serving the built files
RUN apt-get update && apt-get install -y apache2

# Step 8: Copy the build files to the Apache default web directory
RUN cp -rf /app/dist/* /var/www/html/

# Step 9: Expose the port Apache will serve on
EXPOSE 80

# Step 10: Start Apache2 service to serve the frontend application
CMD ["apache2ctl", "-D", "FOREGROUND"]