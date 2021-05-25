# tennessee

## Overview
This is a simple Kubernetes application that simulates network connectivity between three namespaces running on the same cluster. The namespaces are named Nashville, Memphis and Knoxville. Memphis and Knoxville are sibling cities. Nashville is the state capital. The sibling cities have a dependency on the capital.

## Simulated Cloud, Region & Account
The tags 'sim-cloud', 'sim-region' and 'sim-account' are used for simulation purposes. This allows us to simulate multiple clouds, regions and accounts within our single cluster. To accomplish this each deployment.yaml is composed of multiple deployments with these aforementioned tags. For example a podset named frontend will actually be named frontend1, frontend2, frontend...N.

## Network Flows

### City (Memphis & Knoxville)
A city is composed of a frontend and a backend. The frontend communicates with the backend and the backend communicates with the database running in Nashville. No other flows should be permitted. For clarification the frontend for a given city should only communicate with the backed located in the same city. Frontends should not communicate with each other. Backends should not originate connections to anywhere except the database in Nashville.

### Capital (Nashville)
Nashville simulates a database cluster. It has a single deployment called database. Database podsets communicate with other database podsets and accept inbound from city backends.

## Policy

### Infrastructure
A policy is included that will permit any workload running on the cluster to access the cluster DNS (Kube DNS). This demonstrates how infrastructure policies need only be created once and then inherited by new applications.

### SecOPS / Org
A policy is included that will restrict flows from sim-account=10 to sim-account=30. This demonstrates how the SecOPS team can create policy that is inherited by children and can not be overridden.

### Application
Policy configuration for each namespace is provided. The configuration for Memphis and Knoxville is virtually identical.

### Rogue Traffic
Rogue traffic can be generated by running the script or scripts in the directory $repo/tennessee/k8s/memphis/rogue or $repo/tennessee/k8s/knoxville/rogue.

### Deployment
1. Clone this repot (git clone https://github.com/aporeto-se/k8s-simple-apps.git)
1. Change directory into 
