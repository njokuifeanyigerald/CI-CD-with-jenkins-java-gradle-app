# CI/CD workflow 

## Tools I Used

-  **Github** as repo source
- **Jenkins** for continous Integration
- **Helm** is a package manager for Kubernetes which deploys charts, which you can think of as a packaged application
- **Gradle** for building the java app
- **sonarQube** inorder to collects and analyzes source code, and provides reports for the code quality of this project
- **Nexus** is a repo that supports many artifact formats, like Docker, Java™
- **Docker** inorder to containerize the app
-  **Kubernetes** inorder to orchestrate the containerized app
- **Datree.io** is used to identify misconfiguration in k8s manifest files


This application is java spring boot web application  

build tool is `gradle`

when we build the code using command ```./gradlew build ``` it will generate war file. that war can be placed in tomcat server to see application web page

code is integrated with sonarqube plugin which help us in static code analysis 

``` ./gradlew sonarqube ```


**For Creating secret**

*registry secret is used because it is in `deployment.yaml` under imagePullSecrets within the helm chart myapp folder*
```xml
kubectl create secret docker-registry registry-secret --docker-server=127.0.0.1:8083 --docker-username=admin --docker-password=nexus --docker-email=<email>
```