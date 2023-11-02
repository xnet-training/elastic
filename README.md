# elastic
Material de ejercicios sobre Elastic

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
