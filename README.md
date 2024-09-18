# Setting Up Tekton

### Prerequisite

→ Default storage class available in the cluster to provision volumes dynamically.

→ Deploy `openebs` to dynamically provision `hostpath`

```jsx
helm repo add openebs https://openebs.github.io/openebs
helm install openebs --namespace openebs openebs/openebs --create-namespace
```

### Deploy Tekton in kubernetes

```jsx
kubectl apply -f https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

→ Check `kubectl get all -n tekton-pipelines` to see what Tekton deployed

### Install Tekton CLI client in ubuntu

```jsx
wget https://github.com/tektoncd/cli/releases/download/v0.38.0/tektoncd-cli-0.38.0_Linux-64bit.deb
```

→ Tekton will use same auth configuration as `kubectl` used

### Interact with tekton-ci

-> List task

```
tkn t list
```

-> Apply pipeline

```
kubectl apply -f base/instavote-ci-pipeline.yaml

tkn p list
tkn p describe instavote-ci
```

# Interacting with PipelineRun

→ `pipeline` is very generic and could be used by any application by providing its own specific inputs by create `pipelineRun`

→ PipelineRun

→ `spec`

→ `pipelineRef`

→ `workspaces`

→ `volumeClaimTemplate`

→ `secret` → for auth

→ `params`

→ Edit file `vote-ci-pipelinerun.yaml` make some changes

→ `storageClassName: openebs-hostpath`

→ Create `secret generic`  exactly as `docker config.json` not using `docker-registry` type. Because kaniko mount it as it is, not use directly by kubernetes, that as different format

→ run the pipelinerun

→ `kubectl apply -f vote-ci-pipelinerun.yaml`

→ list the pipelinerun `tkn p list` and `tkn pr list`

→ `STATUS` → `Running`

→to track logs pipelineRun `tkn pr logs -f --last`

→ Need to fix some `params` as i use latest version `task` in `instavote-ci Pipeline`

→ `kaniko task`

→ EXTRA_ARGS as an array `["--skip-tls-verify"]`

→ `verify-digest task`

→ `result.IMAGE-DIGEST` → `result.IMAGE_DIGEST`

→ Monitor `pipelineRun` logs

→ `tkn pr logs -f --last vote-ci`

