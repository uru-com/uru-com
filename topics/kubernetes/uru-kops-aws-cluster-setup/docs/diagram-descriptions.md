# Diagram Descriptions

This file provides a brief explanation of each architecture image used in the repository.

## End-to-End Kops Flow
- Shows the full Kops lifecycle from local CLI setup through AWS resources, cluster creation, and validation.
- Includes AWS CLI, Route53, S3 state store, EC2 instances, and the Kubernetes control plane.

## Cluster Creation Flow
- Illustrates the sequential steps required to define, review, and apply a Kops cluster spec.
- Highlights the interaction between `kops create cluster`, `kops edit cluster`, and `kops update cluster`.

## Kops vs kubectl
- Compares the responsibilities of Kops and kubectl in the Kubernetes provisioning workflow.
- Emphasizes that Kops builds and manages the cluster, while kubectl operates on workloads inside the cluster.

## Practice vs Production Architecture
- Describes the differences between a practice cluster and a production cluster.
- Covers environment separation, security, scaling, and infrastructure best practices.
