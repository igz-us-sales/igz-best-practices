## Iguazio Best Practices
* TOC
{:toc}

## MLRun vs Nuclio
### Nuclio 
- Serverless Functions framework
- Intended to provide Real-Time Live Endpoints that can easily connect through Triggers to different Sources (Like Kafka, Streams, HTTP, Cron, ...)

### MLRun
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

### MLRun vs Nuclio       
- While `MLRun` and `Nuclio` are quite interconnected, it does not mean they are interchangable
- For example, a `MLRun` function can have a `Nuclio` runtime
    - This means that a `Nuclio` serverless function is being deployed to the cluster via `MLRun`
    - The same `MLRun` function could also deploy a `Job` or `Dask` runtime instead of a `Nuclio` runtime
- `MLRun` is the orchestrator whereas `Nuclio` is the runtime (or type of function)

## Project Organization
### General Pipeline Directory Structure
This is the general directory structure for a Kubeflow Pipeline project. This structure makes some assumptions on how things are set up in code, particularly the `artifact_path`. This file structure can be seen in action within the [Project Templates](#Project-Templates).
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
- [Platform Deployment](https://github.com/igz-us-sales/igz-platform-deployment) - From Iguazio Platform
- [Remote Deployment](https://github.com/igz-us-sales/igz-remote-deployment) - From Local Machine

## Resource Allocation
With a MLRun function (regardless of runtime: `job`, `mpijob`, `dask`, `nuclio`, etc.), you can allocate resources to the function.
- A `request` is the resources that the job is guaranteed and will not be below. Essentially the lower bound.
- A `limit` is the upper bound of the resources for the job and cannot go above.

Assign resources with the following syntax:
```
fn.with_requests(cpu="125m", mem="32Mi")
fn.with_limits(cpu="250m", mem="64Mi", gpus="1", gpu_type='nvidia.com/gpu')
```

## Troubleshooting and Monitoring
Visit the [Iguazio Troubleshooting Guide](https://igz-us-sales.github.io/igz-troubleshooting/) for more information.
