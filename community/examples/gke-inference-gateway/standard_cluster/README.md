# GKE Inference Gateway Example

This example demonstrates how to deploy a large language model (LLM) on Google Kubernetes Engine (GKE) with the GKE Inference Gateway using a Cluster Toolkit blueprint.

This deployment is based on the official [Getting started with an Inference Gateway](https://gateway-api-inference-extension.sigs.k8s.io/guides/) guide. The blueprint uses modules to create the necessary infrastructure and applies Kubernetes manifests directly from the official `gateway-api-inference-extension` repository to ensure they are always up-to-date.

## Prerequisites

1.  A Google Cloud project with billing enabled.
2.  Sufficient quota for H100 GPUs.
3.  A Hugging Face account and an access token with access to `meta-llama/Llama-3.1-8B-Instruct`.
4.  `gcloud`, `kubectl`, and `helm` CLIs installed and configured.
5.  The Cluster Toolkit binary (`gcluster`) installed.

## Setup

1.  **Configure the blueprint:**

    Open the `gke-inference-gateway.yaml` file and update the following variables in the `vars` section:

    *   `project_id`: Your Google Cloud project ID.
    *   `hf_token`: Your Hugging Face access token.
    *   `authorized_cidr`: The IP address range of the machine you are using to deploy the blueprint (e.g., `YOUR_IP/32`). This is required for the toolkit to access the GKE cluster's API server.

2.  **Deploy the blueprint:**

    Run the following command from the root of the `cluster-toolkit` directory:

    ```shell
    ./gcluster deploy community/examples/gke-inference-gateway/standard_cluster/gke-inference-gateway.yaml
    ```

    The deployment process will take several minutes as it provisions the GKE cluster, node pool, and all the necessary Kubernetes resources.

3.  **Send an inference request:**

    Once the deployment is complete and the gateway has an IP address, you can send a request to the model.

    First, get the credentials for your new cluster:

    ```shell
    gcloud container clusters get-credentials inference-gateway --region us-central1 --project YOUR_PROJECT_ID
    ```
    *(Replace `YOUR_PROJECT_ID` with your actual project ID)*

    Next, get the external IP address of the gateway:

    ```shell
    IP=$(kubectl get gateway/inference-gateway -o jsonpath='{.status.addresses[0].value}')
    PORT=80
    ```
    *(It may take a few minutes for the IP address to be assigned)*

    Then, send a request using `curl`:

    ```shell
    curl -i -X POST http://${IP}:${PORT}/v1/completions \
    -H "Content-Type: application/json" \
    -d '{ \
        "model": "food-review-1", \
        "prompt": "Write as if you were a critic: San Francisco", \
        "max_tokens": 100, \
        "temperature": 0 \
    }'
    ```

## Cleanup

To avoid incurring charges, destroy the resources created by the blueprint.

```shell
./gcluster destroy inference-gateway
```
This command will delete the GKE cluster and all other resources provisioned by the blueprint.
