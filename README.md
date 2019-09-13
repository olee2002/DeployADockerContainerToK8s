# DeployADockerContainerToK8s
describe the process of deploying a docker container to kubenets

## GCloud Initial 
Create an google cloud account with a $300 credit. 
- create a project (Project-ID will be important and will be mentioned below.)
- create a cluster ( run connect script in the console)
- create a spanner instance

```
gcloud init
```

```
gcloud auth login
```
```
gcloud components list
```
```
gcloud config set project PROJECT_ID
```
below can be copy and paste directly from the gcloud console
```
gcloud container clusters get-credentials react-spring-app --zone us-central1-a --project react-spring-app-252019
```

now gcloud CLI is set up!

## Docker
```
gradle build
```
build file is ready (app.jar)

Create docker file to the root directory (example below)
```
FROM openjdk:8-jdk // this will pull image from docker hub (takes some time to download)
COPY build/libs/demo-0.0.1-app.jar /app.jar //(copy local app.jar file to docker container)
EXPOSE 8080
ENTRYPOINT [ "java", "-jar", "/app.jar"] //tell docker which file to exec.
```

## Connect Local Docker to Cloud 

"docker build" will grab the local docker file & "-t" will tag and create an image in the cloud repository with the name provided.

Adding tag after the image will differenciate each deployment! GCloud doesn't recognize the update unless the tag is changed.

```
docker build -t gcr.io/Project-ID/ImageName:Tag .
example : docker build -t gcr.io/react-spring-app-252019/my-spring-app:v2 .
```
push the gcr tagged image to cloud with below
(note: make sure push is done before creating deployment, otherwise it will throw an ImagePullOff err!) 
```
docker push gcr.io/Project-ID/ImageName:Tag
```

## GCloud Deployment
Create a deployment by running (name is "react-spring-app" and image is "gcr.io/react-spring-app-252019/my-spring-app")
images don't need to be directly from gcloud. it can be hosted at dockerhub as well.
### Manually creating deployment & service
```
kubectl run react-spring-app --image=gcr.io/react-spring-app-252019/my-spring-app --port=8080 (creating deployment)
```
```
kubectl expose deployment react-spring-app --type=LoadBalancer --port=80 --target-port=8080
```
### creating deployment from deployment. yml
```
kubectl apply -f ./deployment.yml
```

```
kubectl port-forward react-spring-two-86d56bff9d-prfpp 8080
```
Check the external IP address in GCloud console and should be able to access to the app at this point.

reference:
http://35.226.14.8/gcpAndKubernetes
http://35.226.14.8/deployToKubernetes
http://35.226.14.8/secrets
