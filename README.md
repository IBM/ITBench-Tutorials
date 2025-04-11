# ITBench-Tutorials

## Prerequisites
- [Docker](https://docs.docker.com/get-started/get-docker/)

## Setup kubeconfig (TODO)

## Running with Docker
The agent should always be run in a container in order to prevent harmful commands being run on the user's PC.  

1. Clone the repo.
```bash
git clone https://github.com/IBM/ITBench-SRE-Agent/
cd ITBench-SRE-Agent
```

2. Move the provided kubeconfig into the root directory of this repo.

3. Build the image.
```bash
docker build -t itbench-sre-agent .
```

4. Run the image in interactive mode:
```bash
# Mac
docker run --mount type=bind,src="$(pwd)",target=/app/lumyn -e KUBECONFIG=/app/lumyn/config -it itbench-sre-agent /bin/bash
```
```bash
# Linux
docker run --network=host --mount type=bind,src="$(pwd)",target=/app/lumyn -e KUBECONFIG=/app/lumyn/config -it itbench-sre-agent /bin/bash
```


5. Grab the observability URL
Inside the docker container run:
```bash
kubectl get Ingress -A
```

Copy the URL.

6. Create a `.env` based on `.env.tmpl` by running:


7. Start the agent:
```bash
crewai run
```
