# RB1 BASE cloud simulation

This repository contains all necessary files to deploy Robotnik RB1 base simulation on kubernetes.

## Features

It includes the following:

- World simulation using gazebo
- RB1 Base simulation spawner
- amcl localization and map server
- move-base navigation
- Web gazebo client
- Web full rviz
- SSH terminal access to dedicated container

## Requirements

In order to deploy this simulation you will need:

- kubernetes cluster >= 1.21
- Access to the 5G-era private registry
- kubectl tool

## Deployment instructions

### Downloading the manifest files

```bash
git clone https://github.com/5G-ERA/rb1_base_simulation_k8s.git
cd rb1_base_simulation_k8s
```



### Constant manifest files

In order to deploy use the **only for the first time** the following command:

```bash
kubectl apply -f first-time/
```

This command deploys the services (including the load balancers) that should be active all the time and not change.

### Deploying the Simulation

In order to deploy the simulation use the following command:

```bash
kubectl apply -f pseudo-chart/
```

### Checking that simulation is fully available

Check the state of the deployment using the following command:

```bash
kubectl get pods
```

Wait until everything should be on state "RUNNING" and READY

### Undeploying the simulation

In order to destroy the simulation use the following command:

```bash
kubectl delete -f pseudo-chart/
```

This will destroy all pods and configuration files. But the services will remain active.

## Destroying the simulation

In order to fully destroy the simulation on the cluster including the loadbalancers use the following commands:

```bash
kubectl delete -f pseudo-chart/
kubectl delete -f first-time/
```

## Interaction with simulation

### Gazebo

For obtaining the gazebo client URL use the following commands

```bash
echo "gazebo client website:"
echo http://$(kubectl get svc robot-gazebo-client-vnc -o jsonpath="{.status.loadBalancer.ingress[0].hostname}")
```

Paste the output on your browser new tab or window

### Rviz

For obtaining the rviz URL use the following commands:

```bash
echo "Rviz client website:"
echo http://$(kubectl get svc robot-rviz-vnc -o jsonpath="{.status.loadBalancer.ingress[0].hostname}")
```

Paste the output on your browser new tab or window

### Terminal

For obtaining the rviz URL use the following commands:

```bash
echo "Rviz client website:"
echo ssh ros@$(kubectl get svc robots-ssh-server -o jsonpath="{.status.loadBalancer.ingress[0].hostname}")
```

Use this url to connect to ssh to and interact using ros comands

The default password is `ros-client-password`





