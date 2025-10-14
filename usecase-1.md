# Use Case

ApplicationSet: [.argocd/usecases/appset-usecase-1.yaml](.argocd/usecases/appset-usecase-1.yaml)

## Premises

### Application Deployment

Uses pure Helm for managing deployments.

### Environment Configuration

Each environment is defined in a separate values.yaml file, which includes an infix to distinguish it.
Example:

```bash
values.<env>.yaml
.values.<env>.yaml
```

However, in some cases, a default value is provided to the service owner instead of being explicitly defined by them. In such situations, we need to supply an environment-specific default value.

### Domain Cluster Consistency

Clusters belonging to the same domain must maintain identical deployment specifications, even if they are separate logical clusters.
Example:

```bash
stg-backend-eks-cluster-1
stg-backend-eks-cluster-2
```

Both belong to the same domain (`backend`) for `stg` environment and therefore must deploy services with exactly the same specifications.

### Cluster-Specific Configuration

Certain clusters may require additional cluster-specific configuration, regardless of domain association.
Example:

- `stg-backend-eks-cluster-1` may require an extra field in its release values:

```bash
clusterID: 1
```

Other clusters may define different configurations as needed.
