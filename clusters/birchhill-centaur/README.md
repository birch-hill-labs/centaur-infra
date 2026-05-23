# ACME Centaur Cluster

This directory is the forkable cluster template for one Centaur deployment. It
keeps the environment-specific surface small: Argo CD bootstrap resources, Helm
values, and any raw manifests that live beside the chart.

Apply `argocd/bootstrap/00-namespaces.yaml` first, then
`argocd/bootstrap/centaur.yaml`.

After cloning from the template, rename this directory if `acme-centaur` is not
the deployment name you want to expose in GitOps paths.
