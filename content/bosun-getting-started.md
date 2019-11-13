# Getting Started with Bosun

To quickly get started with Bosun, install into a kubernetes cluster of 1.13+ via Helm using the following commands

**Step 1**

Ensure a kubernetes namespace and MongoDB are installed

**Step 2**

Download the helm chart

```sh
curl -LO https://github.com/boomerang-io/boomerang.docs/raw/stable/content/bmrg-bosun-1.0.4.tgz
```

**Step 3**

Extract the values.yaml from the helm chart

```sh
helm inspect values bmrg-bosun-1.0.4.tgz > bmrg-bosun-values.yaml
vi bmrg-bosun-values.yaml
```

**Step 4**

Install the helm chart

```sh
helm install --namespace <namespace> -f bmrg-bosun-values.yaml bmrg-bosun-1.0.4.tgz
```


