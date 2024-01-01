# Doku Helm Chart Repository
[![Doku](https://img.shields.io/badge/Doku-orange)](https://github.com/dokulabs/doku)
[![License](https://img.shields.io/github/license/dokulabs/helm?label=license&logo=github&color=f80&logoColor=fff%22%20alt=%22License)](https://github.com/dokulabs/helm/blob/main/LICENSE)
[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/doku)](https://artifacthub.io/packages/search?repo=doku)
[![Helm Version](https://img.shields.io/github/tag/dokulabs/helm.svg?&label=Chart%20Version&logo=helm)](https://github.com/dokulabs/helm/tags)
[![GitHub Last Commit](https://img.shields.io/github/last-commit/dokulabs/helm)](https://github.com/dokulabs/helm/pulse)
[![GitHub Contributors](https://img.shields.io/github/contributors/dokulabs/helm)](https://github.com/dokulabs/helm/graphs/contributors)


[![Helm Test](https://github.com/dokulabs/helm/actions/workflows/helm-test.yml/badge.svg?branch=main)](https://github.com/dokulabs/helm/actions/workflows/helm-test.yml)

## Introduction

Doku is an observability tool for Large Language Models (LLM). This repository contains the Helm chart deploys Doku and its required components, including TimescaleDB, on a Kubernetes cluster.

## Installation

To install the Doku chart with the release name `my-doku`:

```bash
helm repo add dokulabs https://dokulabs.github.io/helm/
helm install doku dokulabs/doku
```
**NOTE**:
> As Doku does not have a built-in visualization UI yet, it is preferred that you set up the `observabilityPlatform` configuration within the [values.yaml](./charts/doku/values.yaml) file. Doing so enables visualization of the LLM Observability data processed by Doku using an external observability platform.

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

### Start sending LLM Observability data to Doku

Once Doku has been installed in your cluster and API Key, You can configure the `dokumetry` [Python](https://github.com/dokulabs/dokumetry-python) and [NodeJS](https://github.com/dokulabs/dokumetry-node) SDKs in your LLM Application. These SDKs are designed to collect and send observability data directly to your instance of Doku, providing valuable insights and metrics to monitor and analyze the performance and usage of your Large Language Models (LLM).

## Contributing

We welcome contributions to the Doku project. Please refer to [CONTRIBUTING](CONTRIBUTING) for detailed guidelines on how you can participate.

## License

Doku is available under the [GPL-3.0](LICENSE).

## Support

For support, issues, or feature requests, submit an issue through the [GitHub issues](https://github.com/dokulabs/helm/issues) associated with this repository.