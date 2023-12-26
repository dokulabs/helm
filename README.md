# Doku Helm Chart Repository
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/doku)](https://artifacthub.io/packages/search?repo=doku)

## Introduction

Doku is an observability tool for Large Language Models (LLM). This repository contains the Helm chart deploys Doku and its required components, including TimescaleDB, on a Kubernetes cluster.

## Installation

To install the Doku chart with the release name `my-doku`:

```bash
helm repo add dokulabs https://dokulabs.github.io/helm/
helm install doku dokulabs/doku
```

## Getting Started post Installation

After the Doku chart is successfully deployed to your Kubernetes cluster, you'll need to generate an API key that can be used by the Dokumetry SDKs to authenticate requests to the Doku platform.

### Generating Your First API Key

To create an API key, follow these steps:

1. Determine the external IP address or hostname of the Doku service. If you're using a LoadBalancer service type, you can find this information by running:

    ```bash
    kubectl get svc -l "app.kubernetes.io/name=doku"
    ```

    Look for the `EXTERNAL-IP` value in the output if service type is LoadBalancer/NodePort. Else port-forward the service locally.

2. Use the following `curl` command to make a POST request to the `/api/keys` endpoint:

    ```bash
    curl -X POST http://<Doku-URL>/api/keys \
    -H 'Authorization: ""' \
    -H 'Content-Type: application/json' \
    -d '{"Name": "Ishan"}'
    ```

    Replace `<Doku-URL>` with the actual external IP address or hostname of your Doku service. If you port-fowareded your service, You will need to specify the port aswell(Default is `9044`)

    **Note**: The `Authorization` header can be set to an empty string (`""`) for the first request since no API keys exist yet. In any following requests to Doku endpoints, you will need to use this api key in the `Authorization` header.

3. The above command will return a JSON response containing your new API key. Here's an example response:

    ```json
    {
        "status": "200",
        "message": "dk*****************",
    }
    ```

    Store the provided API key securely; this is the only time you will see it in full.

## Start sending LLM Observability data to Doku

Once Doku has been installed in your cluster and API Key, You can configure the `dokumetry` Python and Node SDKs in your LLM Application. These SDKs are designed to collect and send observability data directly to your instance of Doku, providing valuable insights and metrics to monitor and analyze the performance and usage of your Large Language Models (LLM).

### Support

For support, issues, or feature requests, submit an issue through the [GitHub issues](https://github.com/dokulabs/helm/issues) associated with this repository.
