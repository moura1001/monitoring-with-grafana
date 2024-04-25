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

## Iniciar Grafana

Deploy:
```
kubectl apply -f deploy/grafana-deployment.yaml
```

Expor serviço:
```
kubectl expose svc grafana
```

Expor porta para o host:
```
kubectl port-forward svc/grafana 3000:3000
```

**Grafana UI:**

Usuário e senha padrão: _admin_
```
http://localhost:3000
```

## Configurar Grafana

1) **Configurar Prometheus como data source**
    - Configurar URL de conexão como _http://prometheus:9090_
    - Clicar em _Save & test_ no final da página

2) **Criar um Dashboard**
    - Acessar a página _http://localhost:3000/dashboards_
    - Criar um novo dashboard e importar a partir do template com id **1860**
    - Selecionar data source do Prometheus e clicar em _Import_

3) **Monitorar métricas**

