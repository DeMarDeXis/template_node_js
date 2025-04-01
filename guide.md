# Guide
## How to initialize _Node.js_ project using DOCKER

### 1. Init [Dockerfile](Dockerfile)
```
FROM node:22-alpine  # base image
WORKDIR /app         # workdir in container
COPY package*.json ./  # copy package.json and package-lock.json
RUN npm install      # install dependencies
COPY . .             # copy all files from current directory to container
EXPOSE 5173          # expose port 5173
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]  # cmd to run app
```

### 2. Create [.dockerignore](.dockerignore) file
```
.idea
.git
node_modules
```

### 3. To clean node_modules(opt.)
```
del /s /q node_modules
```

### 4. Refactor [vite.config.js](vite.config.js)
```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    host: true,
    port: 5173,
    strictPort: true,
    watch: {
      usePolling: true
    }
  }
})
```

### 5. To build docker image
```
docker build -t custom-react-app .
```

### 6. To run docker container
```
docker run -p 5173:5173 custom-react-app
```

# CONCLUSION and development

1. To **_Make_** [docker-compose.yml](docker-compose.yaml) file

2. "Hot reboot"(opt.)   
We can to refactor [vite.config.js](vite.config.js) for better performance
```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    host: true,
    port: 5173,
    strictPort: true,
    watch: {
      usePolling: true
      interval: 1000 <------- to add this
    }
  }
})
```

### Some useful commands:
**Docker**:\

```
docker-compose up

docker logs -f react-dev

docker exec -it react-dev sh

docker stop $(docker ps -aq)
```

### When do I need to rebuild the container
1. When the **dependencies** change
2. When the **dockerfile** change
3. If we have a trouble with the environment

### Development steps
Dockerfile(**_Build_**) --> docker-compose.yaml(_**UP**_)