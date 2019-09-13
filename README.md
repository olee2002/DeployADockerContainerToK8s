# DeployADockerContainerToK8s
describe the process of deploying a docker container to kubenets

## Docker
Add docker file to the root directory (example below)
```
FROM openjdk:8-jdk // this will pull image from docker hub (takes some time to download)
COPY build/libs/demo-0.0.1-app.jar /app.jar //(copy local app.jar file to docker container, run gradle build)
EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "/app.jar"] //tell docker which file to exec.
```
