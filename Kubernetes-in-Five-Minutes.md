The areas that Kubernetes natively accounts for in logical order are:

1. Hardware – Node
2. Orchestration – Deployment, Job, CronJob, StatefulSet, DaemonSet
3. Configuration – ConfigMap, Secret
4. Persistence — PersistentVolumeClaim, PersistentVolume
5. Execution – Pod, ReplicaSet
6. Access Control – Namespace, ServiceAccount, Role, ClusterRole
7. Exposure – Service, Ingress

# Hardware

All software has one fundamental dependency: a machine to run on. We need hardware whether it’s our own computers or someone else's and this abstraction comes in the form of a Node resource type.

1. Node: Represents a logical machine/VM (eg. your computer, ec2 instance, a droplet).

# Orchestration

After we've defined our hardware, we need a plan on how to deploy our application. Will it be a single instance? Do we need maybe two? When we need to update our application, should it be done one by one or all at once? Do we want it to be deployed across all our Nodes? Should it be run once every day at 3 AM? We need to orchestrate the deployment.

1. Deployment: Long-standing workloads (eg. a server)
2. Job: One-off workload (eg. a shell script/database migration task)
3. CronJob: Periodically-run one-off workload (eg. data sync task)
4. StatefulSet: Workloads that require a readable/writable data volume (hard disk) that persists beyond restarts and is not shared amongst other instances of the same application. Use cases include services that implement eventual consistency (eg. transactional databases)
5. DaemonSet: Workloads that should be distributed across all targeted Nodes (eg. log collectors, system monitors, security software)
