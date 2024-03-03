# Despliegue de Demo Devops Java en Kubernetes usando Pipelines

El presente proyecto, representa la implementación de una aplicación funcional en un clúster de Kubernetes, conforme a los requisitos de la prueba técnica de DevOps de Devsu. Utilizando Jenkins como parte de un pipeline de CI/CD, la aplicación se ha desplegado en un entorno local de Kubernetes. Además, se han publicado las imágenes en Docker Hub y usando un repositorio público propio en GitHub.



## Iniciando

### Requisitos Previos

- Clúster de Kubernetes

- Docker instalado localmente para construir la imagen del contenedor y/o acceso a un registro de contenedores para almacenar las imágenes Docker 

- kubectl instalado y configurado para interactuar con el clúster de Kubernetes

- Jenkins instalado y configurado con los plugins necesarios para ejecutar el pipeline

  

### Pipeline de Jenkins

Si desea realizar el despliegue manualmente, ejecute los pasos siguientes:

- Construye las imágenes de los contenedores: Utiliza Maven para construir la aplicación y Docker para construir la imagen del contenedor.
- Configura los secretos: Si la aplicación utiliza secretos, asegúrate de crear los secretos en Kubernetes utilizando los archivos YAML proporcionados.
- Aplica los recursos de Kubernetes: Aplica los archivos YAML de configuración en el clúster de Kubernetes utilizando el comando kubectl apply -f [archivo YAML] para cada archivo de configuración, en el orden adecuado como está en el jenkinsfile.



### Estructura del Repositorio

Clona este repositorio público:

```bash
https://github.com/rmillav201/devop_demo.git
```

'`/config`': Contiene los archivos de configuración de Kubernetes (YAML) necesarios para el despliegue.
'`/docker`': Contiene los archivos Dockerfile y otros archivos necesarios para construir las imágenes de los contenedores.
'`Jenkinsfile`': Contiene la definición del pipeline de Jenkins.

### Pipeline de Jenkins

El pipeline de Jenkins está configurado para realizar las siguientes etapas:

- Verificación SCM: Se verifica el código fuente desde el repositorio Git y se obtiene el identificador del commit.
- Build: Se construye la aplicación utilizando Maven.
- Test: Se ejecutan las pruebas unitarias de la aplicación.
- Docker Build & Push: Se construye la imagen Docker de la aplicación y se sube al registro de contenedores.
- Deploy to Kubernetes: Se despliega la aplicación en Kubernetes utilizando los archivos de configuración proporcionados.



### Consideraciones de Seguridad

#### Uso de Tokens  y Credenciales en Jenkins

- **Seguridad de los Tokens de Jenkins**: Gestionar de forma segura los tokens de autenticación en Jenkins, evitando su exposición en el código fuente.
- **Uso de Vault o Secret Manager**: Considerar utilizar herramientas como Vault o un Secret Manager para almacenar y recuperar credenciales de forma segura durante la ejecución del pipeline.

#### Interacción con Kubernetes

- **Seguridad en la Interacción con Kubernetes**: Utilizar tokens de servicio o certificados de cliente para autenticar Jenkins con el clúster de Kubernetes.
- **Limitar los Privilegios**: Asignar permisos mínimos necesarios a Jenkins para interactuar con Kubernetes, limitando el alcance de cualquier posible compromiso de seguridad.
- **URL del Pipeline de Jenkins**: Tener en cuenta que la URL del pipeline de Jenkins para el Ingress, puede variar según el entorno. En el caso que se usó fue de Minikube, la URL puede ser bajo el formato http://minikube-ip:nodeport.

##### 	<u>Configuración del Cifrado y del Ingress</u>

​		**Cifrado Autogenerado**: Se utiliza un certificado TLS autogenerado para cifrar las comunicaciones. El archivo `devops-tls-secret.yaml` contiene la 		configuración para este certificado.

​		**Certificados SSL**: Los certificados necesarios para el despliegue de la aplciación se encuentran en la carpeta `devop_demo/kubernetes/certs`.

​		**Mapeo del Host en el Ingress**: El host devops.devs está mapeado en el Ingress para enrutar el tráfico hacia la aplicación desplegada en Kubernetes.



#### Interacción con Docker Hub

- **Seguridad en las Credenciales de Docker Hub**: Utilizar tokens de acceso o credenciales de servicio en lugar de credenciales de usuario para interactuar con Docker Hub desde Jenkins.

- **Limitar el Alcance de los Tokens**: Asegurar de que los tokens de acceso utilizados por Jenkins tengan permisos limitados.
- **Rotación de Credenciales**: Establecer políticas de rotación periódica de credenciales para garantizar la seguridad continua de las credenciales almacenadas en Jenkins.

Repositorio Docker Hub: El repositorio de la imagen utilizada para el despliegue y que fue generada desde Jenkins está en https://hub.docker.com/r/rmillav201/devop/tags.



### Referencias

- **Repositorio de Git**: El repositorio de código fuente está disponible en este enlace. Los archivos YAML de configuración para Kubernetes se encuentran en la carpeta `devop_demo/kubernetes/`.
- **Archivos de Reportes de Pruebas**: Los archivos de reportes de pruebas generados por Jenkins y la consola de ejecución del pipeline están disponibles en el repositorio con el nombre `pipeline_and_surefire_reports.zip`.
- **Archivos de Resultados de SonarQube**: Los archivos de resultados de la verificación del código utilizando SonarQube se encuentran en el repositorio con el nombre `sonarqube_results.zip`.
- Videos de Ejecución del Pipeline: Los videos de la ejecución completa de la prueba técnica están disponibles en el repositorio con el nombre `videos_ci_cd.zip`.



## Agradecimientos
Agradezco a Devsu por brindarme la oportunidad de realizar esta prueba técnica. Espero que este proyecto cumpla con las expectativas y demuestre mi capacidad para abordar desafíos de DevOps. Agradezco sinceramente la oportunidad y estoy disponible para cualquier consulta o aclaración adicional.