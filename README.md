# Monitoring with Grafana

## Prerrequisitos

- Docker
- kubectl
- k3d

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
