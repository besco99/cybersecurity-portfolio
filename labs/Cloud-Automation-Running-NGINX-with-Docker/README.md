# Cloud Automation: Running NGINX with Docker

## Overview

This portfolio case study reviews an instructional demonstration of running an NGINX web server in a Docker container on a Kali Linux VM. It also examines how infrastructure as code (IaC) supports repeatable deployments. The demonstration was local; no cloud infrastructure was provisioned.

## Environment and Objectives

- Kali Linux, Docker Engine, the NGINX image from Docker Hub, and Firefox.
- Distinguish an image from a running container.
- Trace the request from the host's published port to the web server inside the container.
- Identify what the start, browser, and stop checks establish.

## Demonstration Reviewed

1. The instructor checked the Docker CLI version, then pulled the NGINX image and listed local images. The version check identified the CLI; the successful pull showed that the client could perform an operation through the engine.
2. The instructor started an NGINX container in detached mode and published host port 8080 to container port 80. Docker's running-container list reportedly showed the instance.
3. Firefox displayed the default NGINX page at `http://localhost:8080`, providing a functional check of the local HTTP path.
4. The instructor stopped the container and checked the running-container list again. Stopping the container did not delete it or its image.

## Security Analysis

Containers package application files and dependencies while sharing the kernel of the system that runs them. A virtual machine runs a separate guest OS. Container portability still depends on a compatible platform and runtime. Docker runs containers; Kubernetes orchestrates containerized workloads. Containers on the same network may be able to communicate, so isolation should be verified rather than assumed.

A successful localhost request does not show that the port is restricted to localhost. The demonstration's port mapping did not specify a host interface. For a local-only test, explicitly bind to loopback and verify the listening address and reachability. For broader use, review network exposure, image provenance, version or digest pinning, and runtime privileges. These are recommendations, not controls verified in the lesson.

## Infrastructure as Code

IaC puts infrastructure definitions under version control so changes can be reviewed and applied consistently. The lesson discussed Terraform and AWS CloudFormation for provisioning and Ansible for configuration automation. It did not run any of them. Version history helps reconstruct prior configuration, but reverting code does not guarantee restoration of infrastructure or data.

## Results and Limitations

The supplied lesson reports a running container, a successful local HTTP response, and confirmation that the container was no longer running after it was stopped. This write-up does not claim independent execution, external reachability testing, TLS configuration, image scanning, or production readiness. No transcript, screenshots, or terminal output are published.

## Skills and Takeaways

- Interpret image and container lifecycle checks.
- Explain port publishing and distinguish functional validation from exposure testing.
- Compare container isolation with virtual machines.
- Recognize where IaC review and runtime verification complement each other.

Source: User-supplied cloud automation lesson, summarized in original language.

Reference: [Docker Docs — Port publishing and mapping](https://docs.docker.com/engine/network/port-publishing/).
