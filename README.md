# Elastic Stack
Material de ejercicios sobre Elastic

## Requisitos Básicos

Se requiere tener instalado en las PC los siguientes productos

| Producto | Version | Punto de Descarga |
|-|-|-|
|Oracle Virtual Box o Similar| v7.0.12 | https://download.virtualbox.org/virtualbox/7.0.12/VirtualBox-7.0.12-159484-Win.exe |
|Visual Studio Code|v1.82.2|https://code.visualstudio.com/sha/download?build=stable&os=win32-x64-user|
|Oracle NetBeans|v8.2|https://www.jetbrains.com/datagrip/download/download-thanks.html?platform=windows|

Se requiere tener creada una Virtual Machine con los siguientes productos:

1. Sistema Operativo|virtualbox centos 8 / Ubunto 22.04
2. Docker: [GUia de Instalación](https://docs.docker.com/engine/install/ubuntu/)
3. Docker Compose: [Guia de Instalación](https://docs.docker.com/compose/install/standalone/)
4. Cliente Git: [Guia de Instalación](https://www.digitalocean.com/community/tutorials/how-to-install-git-on-ubuntu-22-04)

## Iniciar Plataforma

```sh
docker-compose up -d
```

## Acceso a Kibana

Acceder a `http://localhost:5601/`:


|**Parametro**|**Valor**|
|-|-|
|Username|elastic|
|Password|changeme|

## APM de Microservicio

```sh
java -javaagent:../elastic-apm-agent-1.43.0.jar \
  -Delastic.apm.service_name=consumerloan \
  -Delastic.apm.secret_token= \
  -Delastic.apm.server_url=http://172.17.8.220:8200 \
  -Delastic.apm.environment=dev \
  -Delastic.apm.application_packages=com.crossnetcorp.banking \
  -jar microservice.jar
```

### Ejecucion de Filebeat (Windows)

> file: D:\01-CROSSNET\01-PROJECTS\05-CajaCusco\filebeat-8.10.4-windows-x86_64\custom\filebeat.yml

```yaml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
    - D:\opt\microservices\consumerloan\application.log

output.logstash:
  # The Logstash hosts
  hosts: ["172.17.8.220:5044"]
```

```dos
D:\01-CROSSNET\01-PROJECTS\05-CajaCusco\filebeat-8.10.4-windows-x86_64\custom> ..\filebeat.exe run . -e --v
```

### Ejecucion DOS

```dos
java -javaagent:../elastic-apm-agent-1.43.0.jar -Delastic.apm.service_name=consumerloan -Delastic.apm.secret_token=ffff -Delastic.apm.server_url=http://172.17.8.220:8200 -Delastic.apm.environment=dev -Delastic.apm.application_packages=com.crossnetcorp.banking -Dspring.datasource.url=jdbc:mysql://172.17.8.220:3306/customerloan -Dspring.datasource.username=microservicio -Dspring.datasource.password=secr3t! -Dspring.flyway.enabled=false -Dspring.cloud.vault.token=root -jar .\api\target\microservicio-api.jar --spring.profiles.active=dev --spring.config.location=file:api\src\main\resources\application-dev.yaml
```
