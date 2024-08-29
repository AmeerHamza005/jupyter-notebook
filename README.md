# jupyter-notebook
To create a "Hello World" application using Docker and a development container with Jupyter Notebook, we can follow these steps:

1. **Create a Dockerfile**: This file will specify the base image and the steps required to set up the development environment.
2. **Build the Docker image**: Using the Dockerfile, we will create an image.
3. **Run a Docker container**: We will run the image as a container and ensure it has Jupyter Notebook installed and running.
4. **Access Jupyter Notebook**: Finally, we'll connect to the running Jupyter Notebook server from a web browser.

Let's go through each step:

### Step 1: Create a Dockerfile

First, create a new directory for your project:

```bash
mkdir hello-world-jupyter
cd hello-world-jupyter
```

Inside this directory, create a file named `Dockerfile` with the following content:

```Dockerfile
# Use the official Python image from the Docker Hub
FROM python:3.12-slim

# Install Jupyter Notebook
RUN pip install notebook

# Set the working directory
WORKDIR /app

# Copy all files from the current directory to /app inside the container
COPY . /app

# Expose the Jupyter Notebook port
EXPOSE 8888

# Run Jupyter Notebook with the following options
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
```

### Step 2: Build the Docker Image

Now, build the Docker image using the Dockerfile:

```bash
docker build -t hello-world-jupyter .
```

This command tells Docker to build an image from the Dockerfile in the current directory (`.`) and tag it as `hello-world-jupyter`.

### Step 3: Run the Docker Container

Once the image is built, you can run a container from this image:

```bash
docker run -p 8888:8888 -v $(pwd):/app hello-world-jupyter
```

- `-p 8888:8888` maps port 8888 on your local machine to port 8888 in the Docker container, allowing you to access the Jupyter Notebook.
- `-v $(pwd):/app` mounts the current directory on your local machine to the `/app` directory inside the container, so any files you create in Jupyter Notebook will be saved to your local machine.

### Step 4: Access Jupyter Notebook

After running the above command, Docker will start the container, and you should see output indicating that Jupyter Notebook is running. It will look something like this:

```
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-1-open.html
    Or copy and paste one of these URLs:
        http://(container-id or 127.0.0.1):8888/?token=some-generated-token
```

Open a web browser and navigate to `http://localhost:8888`. You will be prompted for a token, which you can find in the Docker output (after `token=`).

### Step 5: Create a "Hello World" Jupyter Notebook

1. Once inside Jupyter Notebook, click on "New" in the top right corner and select "Python 3" to create a new notebook.
2. In the first cell of the new notebook, type the following Python code:

   ```python
   print("Hello, World!")
   ```

3. Run the cell by pressing `Shift + Enter`. You should see the output `Hello, World!`.

And that’s it! You've successfully created a "Hello World" application using Docker and a Jupyter Notebook container.
### step 6: Create the .devcontainer Directory:

In your project directory, create a .devcontainer folder. This folder will hold all the configuration files necessary for your dev container setup.

### step 7 **Create `devcontainer.json` in `.devcontainer`**

   This file configures the development container.

   **`.devcontainer/devcontainer.json`**

   ```json
   {
     "name": "Python Dev Container",
     "build": {
       "dockerfile": "Dockerfile"
     },
     "settings": {
       "terminal.integrated.shell.linux": "/bin/bash"
     },
     "extensions": [
       "ms-python.python"
     ],
     "forwardPorts": [8888],
     "postCreateCommand": "python --version",
     "remoteUser": "root"
   }
   ```

   **Explanation of `devcontainer.json` Fields:**

   - `"name"`: Name of your dev container.
   - `"build"`: Specifies the Dockerfile used to build the dev container image.
   - `"settings"`: VS Code settings for the container environment.
   - `"extensions"`: List of VS Code extensions to install in the container.
   - `"forwardPorts"`: List of ports to forward from the container to the host machine (8888 for Jupyter).
   - `"postCreateCommand"`: Command to run after the container is created (e.g., to check Python version).
   - `"remoteUser"`: Specifies the user to use within the container.
