# AWS EC2 PySpark - Jupyter Docker

**IMPORTANT**: This has to run under python 3.7

**Doc**: **[Enlace](https://docs.aws.amazon.com/glue/latest/dg/release-notes.html)**

**Docker HUB**: **[Pull: sandbox-jupyter-spark](https://hub.docker.com/repository/docker/rafafrdz/sandbox-jupyter-spark)**

### **Build docker image**

On the root folder

```python
docker build -t <USER>/sandbox-jupyter-spark ./image/
```

Or, on the image folder where is allocated Dockerfile

```python
docker build -t <USER>/sandbox-jupyter-spark .
```

### **Launch an interactive shell into the Docker container**

On the folder where docker-compose.yml is allocated

```python
docker-compose run --rm -p 8888:8888 sandbox /bin/bash
```

### **Jupyter Command**

```python
jupyter lab --ip 0.0.0.0 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''
```

# Usage

## Add id_rsa file

In order to make a correct connection with the EC2 endpoint, we must to have a copy of the ssh private key into the **/ssh** folder named **id_rsa**

## Launch Jupyter

**Important**: Expose the ports.

On the folder where docker-compose.yml is allocated

```python

docker-compose run --rm -p 8888:8888 -p 8998:8998 sandbox jupyter lab --ip 0.0.0.0 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''
```

It launchs a Jupyter Notebook framework with Python and PySpark. To go into Jupyter, we have to:

Open a browser > type [**localhost:8888**](http://localhost:8888/)

## SSH Connection

After that, we have to do the ssh connection by a terminal into the Jupyer framework (browser)

```bash
ssh -i id_rsa -NTL 8998:XXX.XXX.XX.X:8998 glue@DEV_ENDPOINT_PUBLIC_ADDRESS
```

## Create a new notebook

That’s it!

# Other commands

**Drop Dockers containers**

```python
docker rm -f $(docker ps -aq)
```