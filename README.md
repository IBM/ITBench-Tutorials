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

1. Move the provided kubeconfig into the root directory of this repo.

2. Build the image.
```bash
docker build -t itbench-sre-agent .
```

3. Run the image in interactive mode:
```bash
# Mac
docker run --mount type=bind,src="$(pwd)",target=/app/lumyn -e KUBECONFIG=/app/lumyn/config -it itbench-sre-agent /bin/bash
```
```bash
# Linux
docker run --network=host --mount type=bind,src="$(pwd)",target=/app/lumyn -e KUBECONFIG=/app/lumyn/config -it itbench-sre-agent /bin/bash
```


4. Grab the observability URL
Inside the docker container run:
```bash
kubectl get Ingress -A
```

Copy the URL.

5. Create a `.env` based on `.env.tmpl` by running:
```bash
cp .env.tmpl .env
```
.env guide:
```python
### Embedding (used to enable CrewAI memory feature - Optional) ### 
MODEL_EMBEDDING="" # embedding model name, e.g. text-embedding-3-large
URL_EMBEDDING="" # embedding model url, e.g. https://xxxxx.azure-api.net/openai/deployments/text-embedding-3-large-1/embeddings?api-version=2023-05-15
API_VERSION_EMBEDDING="" # embedding model api version, e.g. 2023-05-15 (same as the end of the url)

### Agent - configures that backend used by the agent ### 
PROVIDER_AGENTS="" # agent model provider, e.g. watsonx, azure, anthropic, openai
MODEL_AGENTS="" # agent model or checkpoint name, e.g. ibm/granite-3-2-8b-instruct, gpt-4o, gpt-4o-2024-11-20
URL_AGENTS="" # agent model url, e.g. https://us-south.ml.cloud.ibm.com (no url required for openai)
API_VERSION_AGENTS="" # only required for azure, e.g. 2024-12-01-preview
API_KEY_AGENTS="" # agent api key
REASONING_EFFORT_AGENTS="" # for o1, o1-mini, and o3-mini only, e.g. low, medium, high
SEED_AGENTS=10 # sets the seed for the agent model
TOP_P_AGENTS=0.95 # sets the top p for the agent model
TEMPERATURE_AGENTS=0.0 # sets the temperature for the agent model
THINKING_AGENTS="" # for Claude Sonnet 3.7 and Granite 3.2 only. use anthropic for CS3.7 and wx for G3.2. leave empty to use these models without thinking
THINKING_BUDGET_AGENTS=6000 # for Claude Sonnet 3.7 only. determines the number of thinking token allowed
MAX_TOKENS_AGENTS=16000 # max new tokens the agent model can output per call

### Tools - configures that backend used by the tools ###
PROVIDER_TOOLS="" # see above
MODEL_TOOLS="" # see above
URL_TOOLS="" # see above
API_VERSION_TOOLS="" # see above
API_KEY_TOOLS="" # see above
REASONING_EFFORT_TOOLS="" # see above
SEED_TOOLS=10 # see above
TOP_P_TOOLS=0.95 # see above
TEMPERATURE_TOOLS=0.0 # see above
THINKING_TOOLS="" # see above
THINKING_BUDGET_TOOLS=6000 # see above
MAX_TOKENS_TOOLS=16000 # see above

WX_PROJECT_ID="" # required only when using a watsonx model

# Linux
OBSERVABILITY_STACK_URL="http://<observability-url>/prometheus"
TOPOLOGY_URL="http://<observability-url>/topology"

# Mac
OBSERVABILITY_STACK_URL="http://<observability-url>/prometheus"
TOPOLOGY_URL="http://<observability-url>/topology"

# DO NOT ALTER THESE VALUES
AGENT_TASK_DIRECTORY="config"
SRE_AGENT_EVALUATION_DIRECTORY="/app/lumyn/outputs"
STRUCTURED_UNSTRUCTURED_OUTPUT_DIRECTORY_PATH="/app/lumyn/outputs"
SRE_AGENT_NAME_VERSION_NUMBER="Test"
EXP_NAME="Test"
GOD_MODE="True"
GRAFANA_SERVICE_ACCOUNT_TOKEN="not_required" 
KUBECONFIG="/app/lumyn/config"
```

6. Start the agent:
```bash
crewai run
```
