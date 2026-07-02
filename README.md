# Tienda de Perritos - Pipeline de CI/CD con GitHub Actions y AWS EKS

Este repositorio contiene la configuración completa para el despliegue automatizado de la aplicación Tienda de Perritos. El proyecto implementa una arquitectura de microservicios contenerizados mediante Docker y un flujo continuo de CI/CD que automatiza la entrega en un clúster gestionado de Kubernetes en Amazon Web Services (AWS EKS).

---

## Arquitectura del Proyecto

La aplicación está dividida en tres componentes principales alojados dentro de un espacio aislado (Namespace) de Kubernetes llamado tienda:

1. Frontend: Interfaz web de usuario construida en tecnologías web estándar, expuesta al público mediante un servicio de AWS de tipo LoadBalancer.
2. Backend: API REST que procesa la lógica de negocios de la tienda, conectada internamente con la base de datos.
3. Database (DB): Motor de base de datos relacional MySQL encargado de almacenar la persistencia de la información de forma segura mediante Secrets.

---

## Flujo de CI/CD (GitHub Actions)

El archivo de configuración .github/workflows/deploy.yml define un pipeline automatizado que reacciona de forma inmediata ante cualquier cambio (git push) realizado en la rama principal main.

### Etapas del Pipeline:
1. Checkout del Código: Descarga la versión más reciente del repositorio en el entorno virtual de ejecución (ubuntu-latest).
2. Autenticación en AWS: Configura dinámicamente las credenciales de AWS de forma segura utilizando los secretos guardados en el repositorio (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY y AWS_SESSION_TOKEN).
3. Actualización de Kubeconfig: Conecta de manera remota el entorno de GitHub con el clúster gestionado de AWS llamado devopseks.
4. Despliegue Automatizado (GitOps): Aplica secuencialmente todos los manifiestos de configuración de Kubernetes declarados dentro de la carpeta k8s/:
   - Configuración de variables de entorno y contraseñas (mysql-secret.yaml).
   - Despliegue de Pods y balanceadores (deployment y service para DB, Backend y Frontend).

---

## Estructura de Directorios Clave

* .github/workflows/deploy.yml -> Configuración del Pipeline de Automatización CI/CD.
* k8s/ -> Carpeta contenedora de todos los manifiestos YAML de Kubernetes.
* frontend/, backend/, db/ -> Código fuente y archivos Dockerfile de cada microservicio.

---

## Comandos Útiles de Diagnóstico (Cheat Sheet)

Si necesitas auditar o verificar el comportamiento de la infraestructura desde la terminal de AWS CloudShell, utiliza los siguientes comandos:

### 1. Verificar el Estado de los Contenedores
Para revisar si todos los pods están corriendo (Running) y comprobar la autoreparación (Auto-healing):
```bash
kubectl get pods -n tienda
