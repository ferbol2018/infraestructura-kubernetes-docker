# Infraestructura resiliente con Docker y Kubernetes

## Descripción

Este proyecto implementa un servicio web desplegado mediante contenedores Docker y administrado con Kubernetes.
La infraestructura permite escalar automáticamente según la demanda utilizando Horizontal Pod Autoscaler (HPA) y se gestionó parcialmente mediante Infraestructura como Código con Terraform.

---

## Tecnologías utilizadas

* Docker
* Kubernetes
* Minikube
* Terraform
* Node.js
* Git / GitHub

---

## Estructura del proyecto

```
infraestructura-kubernetes-docker
│
├── app
│   ├── Dockerfile
│   ├── package.json
│   └── server.js
│
├── kubernetes
│   ├── deployment.yaml
│   ├── service.yaml
│   └── hpa.yaml
│
├── terraform
│   └── main.tf
│
└── README.md
```

---

## Construcción de la imagen Docker

```
docker build -t mi-api .
docker run -p 3000:3000 mi-api
```

---

## Despliegue en Kubernetes

```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f hpa.yaml
```

Verificar pods:

```
kubectl get pods
```

Verificar servicios:

```
kubectl get services
```

---

## Escalabilidad automática

Se configuró un Horizontal Pod Autoscaler que escala los pods entre 2 y 10 dependiendo del uso de CPU.

Prueba de carga:

```
kubectl run -i --tty load-generator --rm --image=busybox -- /bin/sh
while true; do wget -q -O- http://mi-api-service; done
```

---

## Resiliencia

Para probar la resiliencia se eliminó un pod manualmente:

```
kubectl delete pod mi-api-XXXX
```

Kubernetes recrea automáticamente el pod para mantener el número de réplicas definido.

---

## Infraestructura como Código

Se utilizó Terraform para definir el deployment de Kubernetes mediante el archivo:

```
terraform/main.tf
```

Comandos utilizados:

```
terraform init
terraform apply
```

---

## Resultados

La aplicación puede escalar automáticamente bajo carga y recuperarse ante fallos, demostrando resiliencia y gestión automatizada de infraestructura.
