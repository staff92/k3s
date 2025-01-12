# k3s

K3s setup

## Ansible

https://github.com/k3s-io/k3s-ansible

```
ansible-playbook playbooks/site.yml -i inventory.yml --ask-pass -e "extra_server_args=--disable traefik --disable servicelb --disable metrics-server"
```

## Helm repo 

```
helm repo add sealed-secrets https://bitnami-labs.github.io/sealed-secrets

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo add grafana https://grafana.github.io/helm-charts

helm repo add gitea-charts https://dl.gitea.com/charts/

helm repo add uptime-kuma https://helm.irsigler.cloud
```

## Helm install

```
helm install prometheus prometheus-community/kube-prometheus-stack -f prometheus.yml

helm install homepage jameswynn/homepage -f hompage.yml

helm install sealed-secrets -n kube-system --set-string fullnameOverride=sealed-secrets-controller sealed-secrets/sealed-secrets -f sealed-secrets.yml

helm install tetragon ${EXTRA_HELM_FLAGS[@]} cilium/tetragon -n kube-system -f tetragon.yml

helm install gitea gitea-charts/gitea -f gitea.yml

helm install kuma uptime-kuma/uptime-kuma --install --namespace monitoring -f kuma.yml

helm install alertmanager-ntfy oci://codeberg.org/wrenix/helm-charts/alertmanager-ntfy --values alertmanager-ntfy.ym

helm install ntfy oci://codeberg.org/wrenix/helm-charts/ntfy --values ntfy.yml
```

## Helm upgrade

```
helm upgrade tetragon cilium/tetragon -n kube-system --version 0.9.0

helm upgrade prometheus prometheus-community/kube-prometheus-stack -f prometheus.yml --version 0.0.1

helm upgrade kuma uptime-kuma/uptime-kuma --install --namespace monitoring -f kuma.yml --version 0.0.1
```

## alertmanager to ntfy

kubectl edit deployment alertmanager-ntfy > remove liveness et readiness probes
