# centaur-acme-infra

Forkable GitOps template for deploying Centaur with the `centaur-acme` example
overlay.

This repo is intentionally public-consumption safe: no private domains, no
fixed IP addresses, no personal data, and no real secrets. It mirrors the shape
of a production Argo CD deployment while leaving environment-specific values as
placeholders.

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
