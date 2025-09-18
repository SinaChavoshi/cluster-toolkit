# GKE Inference Gateway Example

This example deploys a GKE cluster suitable for running the Inference Gateway.

## Deployment

This blueprint will deploy all the necessary infrastructure and Kubernetes resources in a single run.

### Prerequisites

1.  **Hugging Face Token:** You need a Hugging Face access token with permissions to access the `meta-llama/Llama-3.1-8B-Instruct` model. You can get a token from your Hugging Face account settings.

2.  **Authorized IP Address:** For security, the GKE cluster's control plane is only accessible from a specific IP address range. To get your public IP address, you can run the following command in your terminal:
    ```bash
    echo "$(curl -s ifconfig.me)/32"
    ```

### Deployment Instructions

Deploy the blueprint by running the following command, replacing the placeholder values with your own:

```bash
./gcluster deploy community/examples/gke-inference-gateway/standard_cluster/gke-inference-gateway.yaml \
  --vars project_id=YOUR_PROJECT_ID \
  --vars hf_token=YOUR_HF_TOKEN \
  --vars authorized_cidr=YOUR_IP/32
```
