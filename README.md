# Monitoring with Grafana

## Prerrequisitos

- Docker
- kubectl
- k3d

## Configurar cluster

```
mkdir -p /tmp/k3dvol
k3d cluster create k3d-cluster --volume /tmp/k3dvol:/tmp/k3dvol --servers 1
```

## Iniciar servidores

Deploy e criação dos serviços.

Servidor 1:
```
kubectl create deploy node-exporter1 --port=9100 --image=bitnami/node-exporter:latest

kubectl expose deploy node-exporter1 --port=9100 --type=ClusterIP
```

Servidor 2:
```
kubectl create deploy node-exporter2 --port=9100 --image=bitnami/node-exporter:latest

kubectl expose deploy node-exporter2 --port=9100 --type=ClusterIP
```

Servidor 3:
```
kubectl create deploy node-exporter3 --port=9100 --image=bitnami/node-exporter:latest

kubectl expose deploy node-exporter3 --port=9100 --type=ClusterIP
```

## Iniciar Prometheus

Criação de um ConfigMap para evitar modificações diretamente no arquivo de configuração do Prometheus:
```
kubectl create configmap prometheus-config \
    --from-file=prometheus=./config/prometheus.yml \
    --from-file=prometheus-alerts=./config/alerts.yml
```

Deploy:
```
kubectl apply -f deploy/prometheus-deployment.yaml
```

