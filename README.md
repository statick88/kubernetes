# Flask Hello World en Kubernetes con Istio

Este proyecto implementa una aplicación Flask simple desplegada en Kubernetes, utilizando Istio para gestionar el tráfico y mejorar la observabilidad.

## Requisitos Previos

Asegúrate de tener instalados los siguientes componentes:

- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Istio](https://istio.io/latest/docs/setup/getting-started/#download)

## Instalación y Configuración

### 1. Iniciar Minikube

Ejecuta el siguiente comando para iniciar Minikube:

```bash
minikube start
```
2. Instalar Istio
Descarga e instala Istio con el siguiente comando:

```bash
curl -L https://istio.io/downloadIstio | sh -
```
Luego, navega al directorio de Istio y agrega el binario al PATH:

```bash
cd istio-<version>
export PATH=$PWD/bin:$PATH
```
3. Aplicar los CRDs de Istio
Instala los componentes necesarios de Istio utilizando el perfil demo:

```bash
istioctl install --set profile=demo -y
```
4. Construir la Imagen Docker
Construye la imagen Docker de la aplicación Flask. Asegúrate de etiquetar la imagen como statick/flask-hello-world:latest para que coincida con el manifiesto de despliegue:

```bash
docker build -t statick/flask-hello-world:latest .
```
5. Desplegar la Aplicación en Kubernetes
Aplica los archivos de configuración de Kubernetes para desplegar la aplicación, el servicio y las configuraciones de Istio:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f gateway.yaml
kubectl apply -f virtualservice.yaml
```
# Acceso a la Aplicación

La aplicación Flask se ejecuta en el puerto 5000 dentro del contenedor. Istio expone este puerto en el puerto 80 a través del istio-ingressgateway.

Para acceder a la aplicación a través del istio-ingressgateway, ejecuta el siguiente comando para hacer un port-forward:

```bash
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80
```
Luego, abre tu navegador web y visita:

<http://localhost:8080>

# Verificación del Estado

Para verificar que los pods y servicios estén funcionando correctamente, utiliza los siguientes comandos:

```bash
kubectl get pods
kubectl get svc -n istio-system
```
Esto te ayudará a confirmar que los recursos de Kubernetes e Istio se han desplegado correctamente.

# Actualización y Gestión del Despliegue

Si realizas cambios en el código de la aplicación, sigue estos pasos para reconstruir la imagen Docker y actualizar la configuración en Kubernetes:

```bash
docker build -t statick/flask-hello-world:latest .
kubectl apply -f deployment.yaml
```

# Observabilidad

Este despliegue incluye configuraciones para herramientas de observabilidad como Prometheus, Grafana, y Jaeger para monitorizar el estado de la aplicación. Asegúrate de que estos componentes estén instalados en tu entorno Istio para aprovechar las capacidades de monitorización.

# Contribuciones
Las contribuciones son bienvenidas. Si encuentras algún problema o deseas proponer mejoras, por favor abre un issue o envía un pull request.

# Licencia
Este proyecto está bajo la Licencia MIT. Consulta el archivo LICENSE para más detalles.
