# ArgoCD ApplicationSet Implementation Patterns

This repository demonstrates various ArgoCD ApplicationSet patterns for managing applications across different environments and clusters. Each pattern addresses specific organizational needs and scales.

## 1. Static List ApplicationSet

**Use Case**: Small to medium teams with a fixed set of services and predictable deployment requirements.

### Characteristics:
- **Teams**: 1-5 teams
- **Services**: 5-20 services (fixed list)
- **Environments**: Multiple environments (dev, staging, production)
- **Clusters**: Single or small number of clusters
- **Configuration**: Hardcoded service list in ApplicationSet spec

### Implementation:
- Uses `list` generator with predefined service elements
- Simple matrix combination of services and clusters
- Direct path mapping to Helm charts

### Example Files:
- `.argocd/static/list.yaml` - Basic static list implementation
- `helm/` directory - Individual service Helm charts

### When to Use:
- Small development teams
- Stable service portfolio
- Simple deployment requirements
- Limited infrastructure complexity

---

## 2. Static Multi-Cluster ApplicationSet

**Use Case**: Organizations with multiple clusters but a fixed set of services requiring selective deployment.

### Characteristics:
- **Teams**: 1-5 teams
- **Services**: 5-20 services (fixed list)
- **Environments**: Multiple environments across clusters
- **Clusters**: 2-5 clusters with selective deployment
- **Configuration**: Service-specific cluster inclusion/exclusion rules

### Implementation:
- Uses `matrix` generator combining `list` generators for clusters and services
- Advanced selector logic with `exclude`/`include` mechanisms
- Go template support for complex conditional logic
- Template patching for service-specific configurations

### Example Files:
- `.argocd/static/multicluster-with-include-exclude.yaml` - Multi-cluster with selective deployment
- Service-specific Helm parameters and sync policies

### When to Use:
- Multi-cluster environments
- Services with different cluster requirements
- Need for selective deployment control
- Environment-specific service configurations

---

## 3. Dynamic Git-Based ApplicationSet

**Use Case**: Growing organizations with many services that need automatic discovery and deployment.

### Characteristics:
- **Teams**: 5-20 teams
- **Services**: 20-100+ services (auto-discovered)
- **Environments**: Multiple environments
- **Clusters**: Multiple clusters with dynamic selection
- **Configuration**: Git-based service discovery with runtime configuration

### Implementation:
- Uses `git` generator to discover services from repository structure
- Runtime configuration files (e.g., `runtime/stg-backend.yaml`)
- Dynamic cluster selection with include/exclude mechanisms
- Advanced selector logic for deployment control

### Example Files:
- `.argocd/dynamic/multicluster-wiith-include-exclude.yaml` - Git-based discovery with cluster selection
- `.argocd/dynamic/remote-source.yaml` - Runtime configuration-based deployment
- `runtime/stg-backend.yaml` - Service configuration file

### When to Use:
- Large number of services
- Frequent service additions/removals
- Need for automatic service discovery
- Dynamic deployment requirements

---

## 4. Advanced Service-Specific ApplicationSet

**Use Case**: Complex organizations requiring fine-grained control over individual service deployments with service-specific configurations.

### Characteristics:
- **Teams**: 10-50+ teams
- **Services**: 50-200+ services
- **Environments**: Multiple environments with complex requirements
- **Clusters**: Multiple clusters with sophisticated routing
- **Configuration**: Service-specific `.argocd.yaml` files with metadata-driven deployment

### Implementation:
- Uses `git` generator to discover `.argocd.yaml` files
- Service-specific configuration files (`.argocd.yaml`)
- Metadata-driven deployment with template patching
- Advanced cluster selection based on service metadata
- Support for service-specific sync policies, labels, and annotations

### Example Files:
- `.argocd/dynamic/multicluster-with-cluster-selection.yaml` - Cluster selection based on service metadata
- `helm/*/.argocd.yaml` - Service-specific configuration files
- `kustomize/*/*/stg/.argocd.yaml` - Environment-specific service configurations

### When to Use:
- Large, complex organizations
- Services with unique deployment requirements
- Need for fine-grained deployment control
- Complex multi-cluster routing requirements
- Service-specific operational policies

---

## 5. Hybrid Kustomize + Helm ApplicationSet

**Use Case**: Organizations using both Kustomize and Helm for different types of applications.

### Characteristics:
- **Teams**: 5-30 teams
- **Services**: 20-100 services (mixed Kustomize/Helm)
- **Environments**: Multiple environments
- **Clusters**: Multiple clusters
- **Configuration**: Mixed deployment strategies with unified management

### Implementation:
- Supports both Helm charts and Kustomize overlays
- Service-specific configuration files for both deployment types
- Unified ApplicationSet managing different deployment strategies
- Environment-specific overlays and values

### Example Files:
- `helm/` directory - Helm-based services
- `kustomize/` directory - Kustomize-based services
- Mixed `.argocd.yaml` configurations

### When to Use:
- Mixed deployment strategies
- Legacy applications alongside new services
- Different teams using different tools
- Gradual migration scenarios

---

## Implementation Comparison

| Pattern | Complexity | Scalability | Flexibility | Use Case |
|---------|------------|-------------|-------------|----------|
| Static List | Low | Low-Medium | Low | Small teams, fixed services |
| Static Multi-Cluster | Medium | Medium | Medium | Multi-cluster, selective deployment |
| Dynamic Git-Based | Medium-High | High | High | Growing organizations, auto-discovery |
| Advanced Service-Specific | High | Very High | Very High | Complex organizations, fine-grained control |
| Hybrid Kustomize + Helm | High | High | Very High | Mixed deployment strategies |

---

## Getting Started

1. **Choose the appropriate pattern** based on your organization size and requirements
2. **Review the example files** in the corresponding `.argocd/` subdirectories
3. **Customize the configurations** for your specific environment and services
4. **Test with a subset of services** before full deployment
5. **Graduate to more complex patterns** as your organization grows

Each pattern builds upon the previous ones, allowing for gradual adoption and complexity management as your GitOps practices mature.
