# DevOps Lab: Helm & Kustomize

## Описание
Лабораторная работа по развертыванию приложений в Kubernetes с использованием Helm и Kustomize.

## Часть 1: Helm (Prometheus-Grafana)

**Установка:**
```bash
cd prometheus-grafana-helm
helm install promgra ./

## Доступ:
Grafana: http://10.0.2.15:3000
Логин: admin / grafana

## Файлы:
Chart.yaml - описание чарта
templates/ - Kubernetes манифесты
prometheus-deployment.yaml
prometheus-service.yaml
grafana-deployment.yaml
grafana-service.yaml
grafana-configmap.yaml
prometheus-configmap.yaml
prometheus-pvc.yaml
grafana-pvc.yaml

## Часть 2: Kustomize (Flask-Redis)

## Структура:
base/ - базовые манифесты (общие для всех окружений)
dev/ - development окружение (2 реплики, порт 54321)
prod/ - production окружение (5 реплик, порт 54000)

## Kustomize конфигурация:
namePrefix - добавляет префикс dev-/prod- ко всем ресурсам
labels - добавляет метки environment: dev/prod
patches - изменяет replicas, ports, env переменные

## Установка:
cd flask-redis-kustomize

# Dev окружение
kubectl apply -k dev/

# Prod окружение
kubectl apply -k prod/

## Проверка
# Проверить поды
kubectl get pods

# Проверить сервисы
kubectl get services

# DEV окружение (работает)
curl http://10.0.2.15:54321

# PROD окружение (требует настройки сети Minikube)
curl http://10.0.2.15:54000

## Примечание: PROD окружение развернуто корректно (5 подов Running, endpoints подключены).
## Проблема с доступом через LoadBalancer связана с ограничениями Minikube на VirtualBox.
