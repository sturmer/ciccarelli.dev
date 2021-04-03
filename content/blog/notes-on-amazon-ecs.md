---
title: Notes on Amazon ECS
created_at: 2018-01-30
kind: article
---

Here are some notes I've taken while reading [Building Blocks of Amazon ECS](https://aws.amazon.com/blogs/compute/building-blocks-of-amazon-ecs/).

## Regions and Availability Zones (AZs)
AWS is divided into *regions*.
- They are geographically separated
- Each consists of multiple isolated data centers.

A region is divided into (≥ 2) *Availability Zones*.
- AZs are isolated from each other, but inter-connected via fast network connections.
- One AZ can span more data centers. Common failures in one AZ don't affect other AZs in the same region, making outages of a whole region more rare.

To get more availability, deploy a service over multiple AZs.

## Containers
A container virtualizes an operating system, while a Virtual Machine virtualizes physical hardware. This means that a container *is not* a VM.

A (container) *image* is a package that groups
- the code of the application we want to run
- and all the application's dependencies

Images can be deployed on any host machine. The container takes care of the communication with the host.

ECS allows running containerized applications on a cluster of EC2 instances (that are the containers' hosts). ECS works natively with Docker containers.

## Container instances
An ECS instance is a special EC2 instance that
- runs an ECS container agent
- has an IAM policy and role (IAM is Amazon's identity management system)
- is registered into our ECS cluster

An *ECS task* is a group of containers. They logically belong together in order to make up an application/system. Tasks are not created directly, but through a task definition, that declares which containers belong to the task.

The *ECS container agent* is a program that handles communication between the (ECS) scheduler (the component that manages the ECS tasks) and the ECS instances. The *ECS scheduler* decides on which instance a container runs, according to some user-specifiable constraints.

An *ECS cluster* is a group of container instances inside a region (but possibly, and preferably, across multiple AZs).

To register an instance in a cluster, the instance needs an agent running.

## Services
A service is a way of automating the concept of what to run. Conceptually, it looks like: "I want N tasks, defined by task definition D". This recipe is processed by the ECS infrastructure, that makes sure that those tasks are running, possibly restarting them if they go down.
