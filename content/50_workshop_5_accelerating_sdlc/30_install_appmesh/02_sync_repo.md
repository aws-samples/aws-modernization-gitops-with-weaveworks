+++
title = "Sync your repository and Kustomize the Profile"
chapter = false
weight = 302
+++

`eksctl enable profile` pushed changes to our new repo.
Let's fetch them:

```sh
git pull origin master
```

## Kustomize the profile

[Kustomize](https://github.com/kubernetes-sigs/kustomize) lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is.

We will be using kustomize to create more targeted patches that make our code easier to factor, understand, and reuse.

Create kustomization files for base and flux manifests:

```sh
for dir in ./flux ./base; do
  ( pushd "$dir" && kustomize create --autodetect --recursive )
done
```

Create a kustomization file in the repo root:

```sh
cat << EOF | tee kustomization.yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
bases:
  - base
  - flux
EOF
```

Create .flux.yaml file in the repo root:

```sh
cat << EOF | tee .flux.yaml
version: 1
commandUpdated:
  generators:
    - command: kustomize build .
EOF
```

Verify the kustomization is valid by running a dry run apply:

```sh
kubectl apply --dry-run -k . && echo && echo "config is ok :)"
```

Apply your changes via git and call `fluxctl sync` to immediately apply the changes:

```sh
git add -A && \
git commit -m "init kustomization" && \
git push origin master && \
fluxctl sync --k8s-fwd-ns flux
```

Flux is now configured to patch our manifests before applying them to the cluster.
