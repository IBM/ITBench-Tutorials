# ITBench-Tutorials

## Prerequisites
- [Docker](https://docs.docker.com/get-started/get-docker/)


## Running with Docker
The agent should always be run in a container in order to prevent harmful commands being run on the user's PC.  

1. Clone the repo.
```bash
git clone https://github.com/IBM/ITBench-SRE-Agent/
cd ITBench-SRE-Agent
```

2. Move the provided kubeconfig file into the root directory of this repo.
3. Move the provided `.env` file to the root directory of this repo.

4. Build the image.
```bash
docker build -t itbench-sre-agent .
```

5. Run the image in interactive mode:
```bash
# Mac
docker run --mount type=bind,src="$(pwd)",target=/app/lumyn -e KUBECONFIG=/app/lumyn/config -it itbench-sre-agent /bin/bash
```
```bash
# Linux
docker run --network=host --mount type=bind,src="$(pwd)",target=/app/lumyn -e KUBECONFIG=/app/lumyn/config -it itbench-sre-agent /bin/bash
```


6. Grab the observability URL
Inside the docker container run:
```bash
kubectl get Ingress -A
```

Copy the URL.

7. Open the `.env` file in a text editor  
Update the following values:
- `API_KEY_AGENTS`: Provided API key
- `API_KEY_TOOLS`: Provided API key
- `OBSERVABILITY_STACK_URL`: `http:/<observability-url>/prometheus`
- `TOPOLOGY_URL`: `http:/<observability-url>/topology`


1. Start the agent:
```bash
crewai run
```
