# Flask Hello World on Kubernetes with Istio

Este proyecto implementa una aplicación Flask simple desplegada en Kubernetes, utilizando Istio para la gestión del tráfico.

## Requisitos Previos

Asegúrate de tener instalados y configurados los siguientes componentes:

- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Istio](https://istio.io/latest/docs/setup/getting-started/#download)

## Instalación y Configuración

1. **Inicia Minikube**:
```bash
minikube start
```
Instala Istio: Descarga e instala Istio utilizando el siguiente comando:

```bash
curl -L https://istio.io/downloadIstio | sh -
```
Luego navega al directorio de Istio y agrega el binario al PATH:

```bash
cd istio-<version>
export PATH=$PWD/bin:$PATH
```
Aplica los CRDs de Istio:

```bash
istioctl install --set profile=demo -y
```
Construye la imagen Docker:

```bash
docker build -t statick/flask-hello-world .
```

Aplica los archivos de configuración: Asegúrate de que tus archivos deployment.yaml, service.yaml, gateway.yaml y virtualservice.yaml estén correctamente configurados.

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f gateway.yaml
kubectl apply -f virtualservice.yaml
```
## Acceso a la Aplicación

Para acceder a tu aplicación a través del istio-ingressgateway, ejecuta:

```bash
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
```
Luego, abre tu navegador y visita:

[http://minikube-ip:8080](http://minikube-ip:8080)

Reemplaza **minikube-ip** con la IP obtenida al ejecutar minikube ip.

## Verificación de Estado

Para verificar el estado de los servicios y pods en tu clúster, puedes usar los siguientes comandos:

```bash
kubectl get pods
kubectl get svc -n istio-system
```
## Despliegue y Gestión

Para actualizar tu aplicación, puedes hacer cambios en tu código, reconstruir la imagen y volver a aplicar los archivos de configuración.

## Contribuciones

Las contribuciones son bienvenidas. Siéntete libre de abrir un problema o enviar un pull request.

## Licencia

Este proyecto está bajo la Licencia MIT - consulta el archivo LICENSE para más detalles.
