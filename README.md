# k3s

K3s setup

## Ansible

https://github.com/k3s-io/k3s-ansible

```
ansible-playbook playbooks/site.yml -i inventory.yml --ask-pass
```

## Helm repo 

```
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo add grafana https://grafana.github.io/helm-charts

helm repo add gitea-charts https://dl.gitea.com/charts/
```

## Helm install

```
helm install prometheus prometheus-community/kube-prometheus-stack -f prometheus.yml

helm install homepage jameswynn/homepage -f hompage.yml

helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets -f sealed-secrets.yml

helm install tetragon ${EXTRA_HELM_FLAGS[@]} cilium/tetragon -n kube-system -f tetragon.yml

helm install gitea gitea-charts/gitea -f gitea.yml
```

## Helm upgrade

```
helm upgrade tetragon cilium/tetragon -n kube-system --version 0.9.0

helm upgrade prometheus prometheus-community/kube-prometheus-stack -f prometheus.yml
```