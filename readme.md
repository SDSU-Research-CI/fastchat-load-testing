# FastChat API Load Testing
This repo is for load testing the FastChat OpenAI compatible REST API.
Before giving users access to the API, we should have some understanding of the load that it can handle.

The python script `fastchat-api-load-testing.py` is executed via a Kubernetes [indexed job](https://kubernetes.io/docs/tasks/job/indexed-parallel-processing-static/).
This allows the program to divide its input `chat_prompts` among several pods that are executed in parallel to simulate the desired load.

## Setup
You should create a file `env.yaml` based on the provided `env-template.yaml` replacing the value of `api_key` with your FastChat API key.

You must then create a secret in your namespace from the `env.yaml` file like so:

```bash
kubectl -n [your-namespace] create secret generic fastchat-api --from-file=env=env.yaml
```

## Usage
Begin by cloning this repo to your computer.

You can then run the indexed job in your namespace from the root directory of this repo:

```bash
kubectl -n [your-namespace] apply -f manifest.yaml
```

## Monitoring
The FastChat deployment can be monitored for usage via the Nautilus Grafana dashboard for [GPU usage by namespace](https://grafana.nrp-nautilus.io/d/dRG9q0Ymz/k8s-compute-resources-namespace-gpus?orgId=1&refresh=30s&var-namespace=sdsu-llm), filtering by the namespace "sdsu-llm".
The FastChat API server can also be monitored via logs by:

```bash
kubectl -n sdsu-llm logs -f [pod] -c fastchat-api
```
