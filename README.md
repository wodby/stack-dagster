# Dagster application stack for Kubernetes on Wodby

Deploy Dagster with your own assets, jobs and schedules on Kubernetes with
Wodby: your Git repository holds a Dagster project, and Wodby builds it as a
code location that Dagster loads.

This repository defines the Wodby stack manifests and default service
composition for Dagster.

<!-- wodby:generated:start -->

## Stack contract

- [Dagster stack on Wodby](https://wodby.com/stacks/dagster)
- [Browse Wodby application stacks](https://wodby.com/stacks)
- [Dagster stack guide](https://wodby.com/docs/2.0/stacks/catalog/dagster/)
- [Wodby stack documentation](https://wodby.com/docs/2.0/stacks/)
- [Stack manifest reference](https://wodby.com/docs/2.0/stacks/template/)

## Service definitions

- [Dagster service](https://github.com/wodby/service-dagster)
- [Dagster code location service](https://github.com/wodby/service-dagster-code)
- [PostgreSQL service](https://github.com/wodby/service-postgres)

## What's included

| Component / service | Default configuration |
| --- | --- |
| Dagster<br>`dagster` | required; enabled by default; links: `postgres` → `postgres`; workspace loads `code`; runs in the code server |
| Dagster code location<br>`code` | required; enabled by default; built from your repository; links: `postgres` → `postgres` |
| PostgreSQL<br>`postgres` | required; enabled by default |

Enabled optional services are selected by default but can be excluded when an
app is created. Disabled optional services are available but not selected by
default. Required services cannot be excluded.

## Validate the stack manifest

```bash
wodby stack validate-manifest stack.yml --org <org-id>
```

<!-- wodby:generated:end -->

## Deploy this stack

Connect a Git repository started from the
[Dagster boilerplate](https://github.com/wodby/dagster-boilerplate) to the
`code` service: `app/definitions.py` defines your assets, jobs and schedules,
and `requirements.txt` lists the Python packages they need. Dagster itself comes
with the image, at the Dagster service's version.

The stack sets the Dagster service's workspace to load the `app` code location
from the `code` service, and its run launcher to Dagster's `DefaultRunLauncher`,
so runs execute inside the code server, which has your code and packages. Runs
and their events are stored in PostgreSQL. Configure an IO manager that writes
to external storage for outputs that later runs read.

To load more code locations, such as gRPC servers that run elsewhere, add them
to the Dagster service's **Workspace** config next to `app`.

## Maintain a custom version

1. Fork this repository.
2. Edit the stack manifest and `configs/workspace.yaml`.
3. Import the repository as a [Git-backed stack](https://wodby.com/docs/2.0/stacks/create/#create-a-git-backed-stack).

When replacing or renaming a stack service, update every related link target,
derivative reference and the workspace's host. Stack-local names and referenced
service names are distinct identifiers.
