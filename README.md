# Iguazio Platform Best Practices

## Table of Contents
- [MLRun vs Nuclio](#MLRun-vs-Nuclio)
- [Project Organization](#Project-Organization)
- [Resource Allocation](#Resource-Allocation)
- [Troubleshooting](#Troubleshooting)
- [Monitoring](#Monitoring)

## MLRun vs Nuclio
- Nuclio 
    - Serverless Functions framework
    - Intended to provide Real-Time Live Endpoints that can easily connect through Triggers to different Sources (Like Kafka, Streams, HTTP, Cron, ...)
- MLRun
    - Orchestrator for workloads, enabling you to take code (and some definitions if needed) and run it on top of the cluster (even by just pointing to the MLRun API on the cluster from your laptop)
    - Variety of runtimes 
        - Regular Job (as pod)
        - Dask cluster, Spark cluster
        - MPIjob (Horovod for DL Training)
        - Nuclio function and more (pipeline components can be Nuclio functions)
    - MLRun also does the Tracking of code, versions, artifacts, logs, 
         - Orchestrates and exposes them through its UI
    - Many ways to get a function
        - From disk (.py, .ipynb, .yaml)
        - Function marketplace
        - Git repo
        - MLRun DB (can also get runs, models, graphs, metrics, etc.)
- MLRun vs Nuclio       
    - While `MLRun` and `Nuclio` are quite interconnected, it does not mean they are interchangable
    - For example, a `MLRun` function can have a `Nuclio` runtime
        - This means that a `Nuclio` serverless function is being deployed to the cluster via `MLRun`
        - The same `MLRun` function could also deploy a `Job` or `Dask` runtime instead of a `Nuclio` runtime
    - `MLRun` is the orchestrator whereas `Nuclio` is the runtime (or type of function)

## Project Organization
### General Pipeline Directory Structure
```
root
├── README.md
├── Pipeline.ipynb/py                                # Main pipeline notebook/script
├── config.yaml                                      # Optional config file
├── project                                          # Directory for components and KF files
│   └── PipelineComponent1.ipynb/py
│   └── PipelineComponent2.ipynb/py
│   └── ...
│   └── project.yaml                                 # Generated K8s spec for project
│   └── workflow.py                                  # KF pipeline (generated if using .ipynb)
└── pipeline                                         # Directory for run artifacts - Automatically created by MLRun
    ├── 392b2858-6c34-49cf-a646-5f711120d38a         # Generated Run ID
    │   └── run_artifact_data.csv
    │   └── ...
    │   └── run_artifact_model.pkl
    └── ...
```
### Project Templates
Use the following templates as a starting point for your project. They showcase the current best practices for structuring a project in Iguazio.
- Platform Deployment - From Iguazio Platform
- [Remote Deployment](https://github.com/igz-us-sales/igz-remote-deployment) - From Local Machine

## Resource Allocation
With a MLRun function (regardless of runtime: `job`, `mpijob`, `dask`, `nuclio`, etc.), use the following to allocate resources to the function:
```
fn.with_requests(cpu="125m", mem="32Mi")
fn.with_limits(cpu="250m", mem="64Mi", gpus="1", gpu_type='nvidia.com/gpu')
```

## Troubleshooting and Monitoring
Whether something has gone wrong, or you just want to check in on your function, many of the same tools are used.
### UI
- Pipelines
- MLRun
- Nuclio
- Grafana

### Command Line
- kubectl
- MLRun CLI