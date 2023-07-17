# neonman63_platform
neonman63 Platform repository

# Выполнено ДЗ №1

 - [x] Основное ДЗ
 - [x] Разберитесь почему все pod в namespace kube-system восстановились после удаления

kube-apiserver описан в манифестах в файлах /etc/kubernetes/manifests
```
docker@minikube:~$ sudo cat /etc/kubernetes/manifests/kube-apiserver.yaml 
apiVersion: v1
kind: Pod
metadata:
  annotations:
    kubeadm.kubernetes.io/kube-apiserver.advertise-address.endpoint: 192.168.49.2:8443
  creationTimestamp: null
  labels:
    component: kube-apiserver
    tier: control-plane
  name: kube-apiserver
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-apiserver
```
А уже оттуда эти поды управляются напрямую самим kubelet

Под core-dns описан в деплойменте kube-system:
```
$ kubectl describe deployments -n kube-system 
Name:                   coredns
Namespace:              kube-system
CreationTimestamp:      Sun, 09 Jul 2023 18:29:12 +0300
Labels:                 k8s-app=kube-dns
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               k8s-app=kube-dns
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
```

 - [x] Запуск пода web
## В процессе сделано:
- Добавлен Dockerfile для пода web и собран образ, который запушен на DockerHub
- Создан web-pod.yml и применен командой kubectl apply -f web-pod.yml

## Как запустить проект:
- Применить манифест командой kubectl apply -f web-pod.yml
- Запустить форвардинг командой kubectl port-forward --address 0.0.0.0 pod/web 8000:8000

## Как проверить работоспособность:
- Проверить открыв ссылку http://localhost:8000/index.html

 
 - [x] Задание со *
## В процессе сделано:
- Собран контейнер из предложенных исходников, образ запушен на DockerHub.
- Сгенерирован frontend-pod.yml.
- После запуска проверкой лога пода видим, что сервис требует переменные окружения:
```
{"message":"Tracing disabled.","severity":"info","timestamp":"2023-07-16T15:55:30.129782848Z"}
{"message":"Profiling disabled.","severity":"info","timestamp":"2023-07-16T15:55:30.1298189Z"}
panic: environment variable "PRODUCT_CATALOG_SERVICE_ADDR" not set

goroutine 1 [running]:
main.mustMapEnv(0xc0006a20c0, {0xc0758d, 0x1c})
	/src/main.go:208 +0xb9
main.main()
	/src/main.go:124 +0x5be
```
- Для исправления добавлен список env из предложенного frontend.yml в манифест frontend-pod-healthy.yml

## Как запустить проект:
 - Применить манифест командой kubectl apply -f frontend-pod-healty.yml

## Как проверить работоспособность:
 - Проверить лог командой kubectl get logs frontend, должен быть вывод
 ```{"message":"Tracing disabled.","severity":"info","timestamp":"2023-07-16T16:01:39.700512357Z"}
{"message":"Profiling disabled.","severity":"info","timestamp":"2023-07-16T16:01:39.700559396Z"}
{"message":"starting server on :8080","severity":"info","timestamp":"2023-07-16T16:01:39.701448156Z"}
```

## PR checklist:
 - [x] Выставлен label с темой домашнего задания
