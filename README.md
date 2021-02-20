# CKAD
Certified Kubernetes Application Developer (CKAD) preparation
Preparation to CKAD exam for developers. This repo collects
or exersices, documentaion references and labs as well.

## Prerequisites

* Docker knowledge 
* Linux concepts 
* K8S basic 
* Kubernetes local cluster, see [here](create_k8s_cluster.md) 


## Contents

* Core Concepts - 13%
* Multi-container pods - 10%
* Pod design - 20%
* Configuration - 18%
* Observability - 18%
* Services and networking - 13% 
* State persistence - 8%

### Core concepts 

* Kubernetes API primitives [here](core_concepts/k8s_API.md)
* Creating PODs [here](core_concepts/pods.md)
* Namespaces [here](core_concepts/namespaces.md)
* Container configuration [here](core_concepts/container_args.md)
* ConfigMaps [here](core_concepts/configmaps.md)
* Security Contexts [here](core_concepts/security_context.md)
* Resource Requirements [here](core_concepts/resource_requirements.md)
* Secrets [here](core_concepts/secrets.md)
* Service Accounts [here](core_concepts/service_accounts.md)

### Multi-container pods

* Multi-container pods [here](multicontainer_pods/multi-container-pod.md)

### Observability

* Container logging [here](observability/container_logging.md)
* Debbuging in Kubernetes [here](observability/debugging.md)
* Monitoring of applicatons [here](observability/monitoring_application.md)
* Probes [here](observability/probes.md)

### Pod design

* Deployments [here](pod_design/deployments.md)
* Jobs and cronjobs [here](pod_design/jobs_and_cronjobs.md)
* Labels and selectors [here](pod_design/labels_selectors.md)
* Rolling Updates [here](pod_design/rolling_updates.md)

### Configuration

* Configmaps [here](configuration/configmaps.md)
* Resource requirements [here](configuration/resource_requirements.md)
* Secrets [here](configuration/secrets.md)
* Security context [here](configuration/security_context.md)
* Service accounts [here](configuration/service_accounts.md)

### Services and networking 

* Network Policies [here](services_and_networking/network_policies.md)
* Services [here](services_and_networking/services.md)

### State persistence

* PV Volumes and claims [here](state_persistence/persistentVolumes_and_claims.md)
* Volumes [here](state_persistence/volumes.md)
