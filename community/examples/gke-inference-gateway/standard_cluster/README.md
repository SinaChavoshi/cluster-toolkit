# GKE Inference Gateway Example

This example deploys a GKE cluster suitable for running the Inference Gateway.

## Deployment

This blueprint must be deployed in stages by manually commenting out modules in the `gke-inference-gateway.yaml` file.

**Note on `authorized_cidr`:** To get your public IP address for the `authorized_cidr` variable, you can run the following command in your terminal:
```bash
echo "$(curl -s ifconfig.me)/32"
```

### Deployment Instructions

Follow these steps carefully:

1.  **Deploy Infrastructure:** In `gke-inference-gateway.yaml`, comment out the modules with IDs `apply_manifests` and `helm_install_inference_pool`. Then run the deploy command. This will create the network, cluster, and the Hugging Face secret.

    ```bash
    ./gcluster deploy community/examples/gke-inference-gateway/standard_cluster/gke-inference-gateway.yaml \
      --vars project_id=YOUR_PROJECT_ID \
      --vars hf_token=YOUR_HF_TOKEN \
      --vars authorized_cidr=YOUR_IP/32
    ```

2.  **Apply Manifests:** Uncomment the `apply_manifests` module, but keep `helm_install_inference_pool` commented. Run the deploy command again.

    ```bash
    ./gcluster deploy community/examples/gke-inference-gateway/standard_cluster/gke-inference-gateway.yaml \
      --vars project_id=YOUR_PROJECT_ID \
      --vars hf_token=YOUR_HF_TOKEN \
      --vars authorized_cidr=YOUR_IP/32
    ```

3.  **Install Inference Pool:** Finally, uncomment the `helm_install_inference_pool` module and run the deploy command one last time.

    ```bash
    ./gcluster deploy community/examples/gke-inference-gateway/standard_cluster/gke-inference-gateway.yaml \
      --vars project_id=YOUR_PROJECT_ID \
      --vars hf_token=YOUR_HF_TOKEN \
      --vars authorized_cidr=YOUR_IP/32
    ```
