# Flow

Boomerang Flow is a modern, cloud-native workflow automation tool built on top of Kubernetes. Enable new ways of approaching your business tasks or combine with existing tools to extend your current workloads.

Supporting the 'no-code' paradigm with out-of-the-box tasks with the extensibility to bring your own tasks with custom container support. 

Create your next workflow and you will have an end-to-end process that executes a series of tasks based on a Directed Acyclic Graph (DAG).

## Benefits

* Cloud-native from the ground up - no overhead from legacy implementations.
* Cloud agnostic - run in any Kubernetes.
* Each task in the workflow is executed as a pod.
* Visual editor for creating the DAG.
* Trigger your workflows in four easy methods; manual, webhook, schedule, or action listener.
* Multi step workflows supporting parallel processing, decisions, dynamic storage, and parameterization. 
* Workflow change log and versioning with the ability to roll back to any prior revision.
* Activity detail with per execution records detailing the executed workflow and tasks. Logs, status, and output properties are available.
* Dynamic insights over time filtered per team and / or workflow.
* Pre integrated with Boomerang teams and role based access.

## Workflow, aka the DAG

- Steps in a DAG are a task.
- Tasks are a discrete piece of work.
- All of the tasks in DAG make up a workflow.

## Features

### Workflows
Create, view, and manage your workflows from a centralized location. 

### Activity
View the status, logs, outputs, and run times of your recent workflow executions.

### Editor
Advanced visual drag and drop workflow designer. Define triggers, set options, create properties, and link the workflow tasks together.

### Insights
View powerful metrics and real time statistics on workflow executions over time showing peak execution periods, average run times, percentage of success.

### Task Templates
Create, view and manage task templates used in workflows.

## Architecture

![Architecture](./assets/img/Boomerang-Flow-Architecture.png)

The Boomerang Flow application has the following main components

1. Frontend web application - end user visual designer enabling "no-code" workflow building. Ability to see manage all aspects of your workflows including execution, activity and insights.
2. Backend Microservice - translates the requests from the front end and other trigger interactions to an agnostic DAG model.
3. Kubernetes Controller - integrates with Kubernetes to perform and manage executions.
4. Task Workers - containers to execute the tasks mapped in the workflow.

### Standard Tasks

Standard tasks are pre built tasks designed for a no code experience in Flow. They are single focus tasks that may offer tight integration into the platform. They have a guaranteed implementation and tested experience.

### Custom Tasks

![Architecture](./assets/img/Boomerang-Flow-Architecture-CustomTask.png)

The custom task is a slightly different implementation than the standard tasks that come out of the box with Flow. Where as the standard tasks have deep integration to the Flow and Controller services, the custom task has no knowledge or understanding of this, nor do we want to force teams to adhere to a specific implementation.

As such the custom task is essentially a bring your own container paradigm which Flow then wraps with its own lifecycle implementation via init-containers and sidecars.

The custom task is denoted by a flag in the upper left corner of the task in the designer and workflow run screens.

#### Init Lifecycle

The Init Lifecycle is implemented as an init-container and ensures that the pre-start requirements are met. This includes creating a lock file and any other dependencies that are needed.

#### Watcher Lifecycle

The watcher lifecycle is implemented as a sidecar container. This container waits for the custom container to complete and then executes the termination activities. This includes setting output properties for the custom task.

The Watcher and the Custom Task containers are linked through a kubernetes emptyDir mounted to the `/lifecycle` path.

#### Controller

The Flow controller will orchestrate this lifeycle in conjunction with ensuring the normal critera.
