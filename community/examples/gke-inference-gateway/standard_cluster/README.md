# GKE Inference Gateway Example

This example deploys a GKE cluster suitable for running the Inference Gateway.

## Deployment

This blueprint deploys all the necessary infrastructure and software in a single step.

**Note on `authorized_cidr`:** To get your public IP address for the `authorized_cidr` variable, you can run the following command in your terminal:
```bash
echo "$(curl -s ifconfig.me)/32"
```

### Deploy the Blueprint

Run the following command to deploy the entire example:

```bash
./gcluster deploy community/examples/gke-inference-gateway/standard_cluster/gke-inference-gateway.yaml \
  --vars project_id=YOUR_PROJECT_ID \
  --vars hf_token=YOUR_HF_TOKEN \
  --vars authorized_cidr=YOUR_IP/32
```