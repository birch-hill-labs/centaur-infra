<img width="1500" height="500" alt="Centaur banner" src="https://github.com/user-attachments/assets/cc85cdb1-5a72-4eb2-ba1b-2e0a8fbbf691" />

<h4 align="center">
    Forkable GitOps template for deploying Centaur with an organization overlay.
</h4>

<p align="center">
  Bring the Centaur chart, your overlay image, and cluster-specific values
  together through Argo CD.
</p>

<p align="center">
  <a href="#use-this-template">Use This Template</a> •
  <a href="#repositories">Repositories</a> •
  <a href="#bootstrap">Bootstrap</a> •
  <a href="#configure-images">Configure Images</a> •
  <a href="#configure-secrets">Configure Secrets</a>
</p>

## Overview

`centaur-acme-infra` is the companion GitOps template for deploying
[Centaur](https://github.com/paradigmxyz/centaur) with an organization overlay
such as [`centaur-acme`](https://github.com/paradigmxyz/centaur-acme).

It is intentionally public-consumption safe: no private domains, no fixed IP
addresses, no personal data, and no real secrets. It mirrors the shape of a
production Argo CD deployment while leaving environment-specific values as
placeholders.

```text
centaur-acme-infra
    |
    +-- Argo CD application
    +-- Helm values
    +-- optional raw manifests
    |
    v
Centaur in your Kubernetes cluster
```

## Use This Template

1. Click **Use this template** in GitHub, or fork the repo if you want to keep a
   visible upstream relationship.
2. Rename `clusters/acme-centaur` if your environment uses a different cluster
   or app name.
3. Replace `paradigmxyz` repositories and `ghcr.io/paradigmxyz/*` images with
   your organization-owned repositories or mirrors.
4. Create the required `centaur-infra-env` Kubernetes Secret in the target
   namespace.
5. Apply the bootstrap manifests and let Argo CD reconcile Centaur.

## Repositories

- Centaur chart: `https://github.com/paradigmxyz/centaur`
- Example overlay: `https://github.com/paradigmxyz/centaur-acme`
- Overlay image: `ghcr.io/paradigmxyz/centaur-acme`

## Bootstrap

1. Create or choose a Kubernetes cluster.
2. Install Argo CD.
3. Create the required Centaur infrastructure Secret in the target namespace.
4. Apply the bootstrap app:

```bash
kubectl apply -f clusters/acme-centaur/argocd/bootstrap/00-namespaces.yaml
kubectl apply -f clusters/acme-centaur/argocd/bootstrap/centaur.yaml
```

Argo CD then installs the Centaur Helm chart and mounts the ACME overlay image.

## Configure images

Edit `clusters/acme-centaur/argocd/bootstrap/centaur.yaml` and replace:

- `sha-0000000` image tags
- `ghcr.io/paradigmxyz/*` image repositories if you mirror images elsewhere

The template tracks the Centaur chart from `main` by default. Pin
`targetRevision` to a commit SHA for production.

## Configure secrets

Centaur expects an existing Kubernetes Secret named `centaur-infra-env` by
default. The minimum keys are documented in the main Centaur docs. For model
access, store provider keys with the names Centaur expects:

- `OPENAI_API_KEY`
- `ANTHROPIC_API_KEY`
- `AMP_API_KEY`

With iron-proxy and 1Password, those names refer to 1Password item names rather
than raw environment variables in agent sandboxes.

## Layout

```text
clusters/
  acme-centaur/
    argocd/
      bootstrap/   # Argo CD applications and namespaces
      values/      # Helm values for the Centaur app
      apps/        # optional raw manifests managed with the app
```
