# OpenShift GPU Monitoring and Scaling Configuration

This repository contains configurations to monitor and scale GPU-based workloads on OpenShift, particularly for the **vLLM** (Virtual Large Language Model) application. These configurations enable detailed GPU health checks, custom GPU metrics for Horizontal Pod Autoscaling (HPA), and integration with NVIDIA's DCGM Exporter for GPU monitoring.

## Files Included

1. **`vllm_deployment.yaml`**  
   Contains the Kubernetes deployment configuration for **vLLM**, including the enhanced readiness probes for GPU and application health checks.
   
2. **`dcgm_exporter_deployment.yaml`**  
   This YAML file sets up the **NVIDIA DCGM Exporter** to collect GPU metrics from NVIDIA GPUs, which will be used for scaling decisions and monitoring purposes.
   
3. **`prometheus_config.yaml`**  
   Configuration file for **Prometheus** to scrape metrics from **vLLM** and **NVIDIA DCGM Exporter**.
   
4. **`vllm_hpa.yaml`**  
   Horizontal Pod Autoscaler (HPA) configuration to scale the **vLLM** deployment based on custom GPU utilization metrics.
   
5. **`nvidia_scc.yaml`**  
   Security Context Constraint (SCC) configuration for OpenShift to ensure proper permissions for containers utilizing NVIDIA GPUs.

## Prerequisites

- OpenShift 4.x or newer
- NVIDIA GPU with CUDA support (for the DCGM Exporter and GPU monitoring)
- Prometheus set up for metric scraping
- Horizontal Pod Autoscaler (HPA) configured

## Getting Started

### 1. Deploy vLLM

First, apply the **vLLM** deployment configuration. This will create the necessary pods for your workload, along with enhanced readiness probes that check both the health of the vLLM application and the GPU's availability (via CUDA and MIG):

```bash
oc apply -f vllm_deployment.yaml
```

### Step 2: Deploy DCGM Exporter

Next, deploy the NVIDIA DCGM Exporter to monitor GPU metrics. This exporter will expose GPU metrics that can be used for scaling decisions and for monitoring:

```bash
oc apply -f dcgm_exporter_deployment.yaml
```

### Step 3: Configure Prometheus

Prometheus will be used to scrape the metrics from both vLLM and the DCGM Exporter. Apply the Prometheus scraping configuration to start collecting the necessary metrics:

```bash
oc apply -f prometheus_config.yaml
```

### Step 4: Set Up Horizontal Pod Autoscaler (HPA)

```bash
oc apply -f vllm_hpa.yaml
```

### Step 5: Apply Security Context Constraints (SCC)

To ensure the containers in OpenShift have the proper access to GPU resources, apply the Security Context Constraints (SCC) configuration. This will allow the vLLM and DCGM Exporter containers to access the NVIDIA GPU resources:

```bash
oc apply -f nvidia_scc.yaml
```
