# ---------- Stage 1: Build React App ----------
FROM node:18-slim AS build

# Set working directory
WORKDIR /app

# Copy package files first (better caching for Docker)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy rest of the project
COPY . .

# Build the React app for production
ENV REACT_APP_BACKEND_URL=http://13.60.38.103:8080
RUN npm run build


# ---------- Stage 2: Serve with Nginx ----------
FROM nginx:alpine

# Remove default nginx index.html
RUN rm -rf /usr/share/nginx/html/*

# Copy build files from first stage (dist instead of build)
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
