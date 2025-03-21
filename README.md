# Cloud-computing-Assignment-2
## Docker Files with Explanations

## MySQL Dockerfile (database/Dockerfile)

### Explanation:
- Uses `mysql:latest` as the base image.
- Sets environment variables for the root password and database name.
- Copies the `init.sql` script into the `/docker-entrypoint-initdb.d/` directory. MySQL executes this script automatically during initialization to create the `News` table and populate it with data.

### database/Dockerfile:
```dockerfile
FROM mysql:latest
ENV MYSQL_ROOT_PASSWORD=root
ENV MYSQL_DATABASE=urgentnew
COPY init.sql /docker-entrypoint-initdb.d/
EXPOSE 3306
```

## Backend Dockerfile (backend/Dockerfile)

### Explanation:
- Uses `node:16` as the base image.
- Sets up the working directory, installs dependencies from `package.json`, and copies the application code.
- Exposes port `3000` and starts the server using `node app.js`.

### backend/Dockerfile:
```dockerfile
FROM node:16
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```

## Frontend Dockerfile (frontend/Dockerfile)

### Explanation:
- Uses `httpd:latest` (Apache web server).
- Copies the `index.html` file to the default Apache document root.
- Exposes port `80` for serving the webpage.

### frontend/Dockerfile:
```dockerfile
FROM httpd:latest
COPY index.html /usr/local/apache2/htdocs/index.html
EXPOSE 80
```
## Docker Compose File (docker-compose.yml)

This file helps start all three containers together.

```yaml
version: '3.8'

services:
  database:
    image: halaassi/urgentnews-database:latest
    container_name: urgentnews-database
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: urgentnew
    ports:
      - "3307:3306"

  backend:
    image: halaassi/urgentnews-backend:latest
    container_name: urgentnews-backend
    environment:
      DB_HOST: database
      DB_USER: root
      DB_PASSWORD: root
      DB_NAME: urgentnew
      DB_PORT: 3306
    ports:
      - "3000:3000"
    depends_on:
      - database

  frontend:
    image: halaassi/urgentnews-frontend:latest
    container_name: urgentnews-frontend
    ports:
      - "8080:80"
```

## DockerHub Image Links

- **Database Image**: [`halaassi/urgentnews-database:latest`](https://hub.docker.com/r/halaassi/urgentnews-database)
- **Backend Image**: [`halaassi/urgentnews-backend:latest`](https://hub.docker.com/r/halaassi/urgentnews-backend)
- **Frontend Image**: [`halaassi/urgentnews-frontend:latest`](https://hub.docker.com/r/halaassi/urgentnews-frontend)




##  How to Run the Application

### To Build the Images:
```sh
# Build the backend image
docker build -t halaassi/urgentnews-backend:latest .

# Build the database image
docker build -t halaassi/urgentnews-database:latest .

# Build the frontend image
docker build -t halaassi/urgentnews-frontend:latest .
```

### To Push the Images to Docker Hub:
```sh
# Push the backend image
docker push halaassi/urgentnews-backend:latest

# Push the database image
docker push halaassi/urgentnews-database:latest

# Push the frontend image
docker push halaassi/urgentnews-frontend:latest
```

### Running the Application Using Docker Compose:
```sh
docker compose up
```

The video
 


https://github.com/user-attachments/assets/ce8d8e6b-35e0-4b8e-b9dc-d8e28dd1484f




