## A Dockerfile with Both Frontend and Backend

```dockerfile
# Base image for Node.js
FROM node:14-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy backend code
COPY backend/ ./backend/

# Copy frontend code
COPY frontend/ ./frontend/

# Build frontend
WORKDIR /app/frontend
RUN npm run build

# Move back to the root directory
WORKDIR /app

# Expose port for the backend
EXPOSE 3000

# Start the backend server
CMD ["node", "backend/server.js"]
```

The `WORKDIR` instruction sets the working directory for subsequent instructions in the Dockerfile. It does not affect the working directory on the host machine where you're building the Docker image.

1. `WORKDIR /app`: This sets the working directory inside the container to `/app`. Any subsequent instructions, like `COPY` or `RUN`, will be executed relative to this directory (`/app`) within the container.

2. `COPY package*.json ./`: This copies the `package.json` and `package-lock.json` files from the host machine's current working directory (where you're running the `docker build` command) to the container's working directory (`/app`).

3. `RUN npm install`: This runs the `npm install` command within the container's working directory (`/app`) to install the dependencies listed in `package.json`.

4. `COPY backend/ ./backend/`: This copies the entire `backend/` directory from the host machine's current working directory to the container's `/app/backend/` directory.

5. `COPY frontend/ ./frontend/`: This copies the entire `frontend/` directory from the host machine's current working directory to the container's `/app/frontend/` directory.

6. `WORKDIR /app/frontend`: This changes the working directory within the container to `/app/frontend`.

7. `RUN npm run build`: This runs the `npm run build` command within the container's working directory (`/app/frontend`) to build the React frontend.

8. `WORKDIR /app`: This changes the working directory back to `/app` within the container.

So, the `WORKDIR` instruction sets the working directory within the container, but it does not change the working directory on the host machine.

When you run the `docker build` command, it should be executed from the same directory where your `Dockerfile` and the application source code (`backend/` and `frontend/`) are located. This ensures that the `COPY` instructions can find the correct files and directories relative to the host machine's working directory.

For example, if your directory structure on the host machine looks like this:

```
my-app/
├── Dockerfile
├── backend/
│   ├── package.json
│   ├── server.js
│   └── ...
└── frontend/
    ├── package.json
    ├── src/
    │   ├── App.js
    │   └── ...
    └── public/
        ├── index.html
        └── ...
```

You would run the `docker build` command from the `my-app/` directory, like this:

```bash
cd my-app/
docker build -t my-app:v1 .
```

This ensures that the `COPY` instructions in the Dockerfile can find the `backend/` and `frontend/` directories relative to the host machine's working directory (`my-app/`).