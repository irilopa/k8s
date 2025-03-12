# Kubernetes Manifests for Argo CD

Este repositorio contiene una colección de manifiestos de Kubernetes diseñados para su uso con Argo CD. 

## Requisitos

Antes de comenzar, asegúrate de tener los siguientes requisitos cumplidos:

- Un clúster de Kubernetes en funcionamiento.
- Argo CD instalado en el clúster.
- `kubectl` y `argocd` CLI instalados y configurados correctamente.
- Acceso al repositorio de Git donde se alojan los manifiestos.

## Instalación y Uso

### 1. Agregar la aplicación en Argo CD

Ejecuta el siguiente comando para registrar el repositorio en Argo CD:

```sh
argocd repo add https://github.com/irilopa/k8s.git --username tu-usuario --password tu-contraseña
```

### 2. Crear una aplicación en Argo CD

Puedes crear una aplicación en Argo CD usando la CLI o mediante un manifiesto YAML.

#### Usando la CLI:

```sh
argocd app create mi-aplicacion \
  --repo https://github.com/irilopa/k8s.git \
  --path ruta/a/manifiestos \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace mi-namespace
```

#### Usando un manifiesto YAML:

Crea un archivo `app.yaml` con el siguiente contenido:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: mi-aplicacion
  namespace: argocd
spec:
  destination:
    namespace: mi-namespace
    server: https://kubernetes.default.svc
  project: default
  source:
    path: ruta/a/manifiestos
    repoURL: https://github.com/irilopa/k8s.git
    targetRevision: HEAD
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

Aplica el manifiesto en el clúster:

```sh
kubectl apply -f app.yaml -n argocd
```

### 3. Sincronizar la aplicación

Para desplegar la aplicación, usa:

```sh
argocd app sync mi-aplicacion
```

### 4. Monitoreo del estado de la aplicación

Para verificar el estado de la aplicación:

```sh
argocd app get mi-aplicacion
```

## Contribuciones

Las contribuciones son bienvenidas. Si deseas mejorar estos manifiestos o agregar nuevos, por favor abre un issue o envía un pull request.

## Licencia

Este repositorio está bajo la licencia [MIT](LICENSE).

