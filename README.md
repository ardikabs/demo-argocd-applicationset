# Scenario

## Static ApplicationSet

- Involves 1–5 teams.
- Typically handles 0–20 services.
- Targets multiple environments.
- Deployed across a small number of clusters.

## Dynamic ApplicationSet

- Builds on the static approach.
- Involves a larger number of teams.
- Operating 20–100 services typically remains manageable.
- Deployed across several clusters, potentially spanning multiple clusters (multi-cluster).

## Advanced ApplicationSet

- The dynamic/static strategy introduces more moving parts in service deployment.
- To manage complexity, service declarations are decoupled.
- Each service has its own deployment manifest directory. (re: `.argocd.yaml`)

## Multi-Cluster ApplicationSet

- A more sophisticated strategy.
- A single ApplicationSet is used to manage deployments across multiple clusters.
- Designed for multi-cluster orchestration with centralized configuration.
