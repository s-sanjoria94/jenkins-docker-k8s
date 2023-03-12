# Jenkins in Docker-Compose

1. Enable & Validate docker environment & docker-compose setup.

```powershell
choco install -y docker-desktop
choco install -y docker-compose
``` 

2. Update the Docker-compose.yml with volume-mount mappings, and image refs. 

3. Use the existing Docker-compose setup to bring up Jenkins locally on a Docker environment 

```powershell
$env:HOME="C:\Users\<USERNAME>"
docker-compose up -d
docker ps -a
docker container logs jenkins
docker container exec -it jenkins "cat /var/jenkins_home/secrets/initialAdminPassword"
```

4. Give the containers about 5 minutes to spin up Jenkins, you can access it on http://localhost:8080 (not https)

<br>

# Jenkins in Kubernetes
This repository has a `Dockerfile` and Jenkins official `helm` chart for setting up a simple Jenkins master for running in Kubernetes.

This Jenkins has the required tools to work in and with Kubernetes
- Jenkins application with pre-loaded plugins (see [plugins-pack-1.txt](plugins.txt))
- Skipped setup wizard
  - You can control admin user and password with `--set adminUser=${USER},adminPassword=${PASSWORD}`
  - You can add and remove plugins by editing the [plugins-pack-1.txt](plugins.txt) file

<br>

### Deploy Jenkins helm chart to Kubernetes
```bash
# Init helm on your cluster
$ helm init
$ kubectl apply -f rbac-config.yaml

# Deploy the Jenkins helm chart
# (same command for install and upgrade)
$ helm upgrade --install jenkins ./helm/jenkins-k8s
```

# Customize & Build your Jenkins 

- Build & Push your custom Jenkins-nonprod image to DockerHub

``` powershell
# Build the image
docker image build -t bobbybabu007/jenkins-nonprod:<VERSION> -f Dockerfile-nonprod .

# Push the image
docker image push bobbybabu007/jenkins-nonprod:<VERSION>
```
<br>
- Build & Push your custom Jenkins-prod image to DockerHub

```powershell
# Build the image
$ docker image build -t bobbybabu007/jenkins:<VERSION>  -f Dockerfile-prod .

# Push the image
$ docker push bobbybabu007/jenkins:<VERSION>
```

# Data persistence
By default, in Kubernetes, the Jenkins deployment uses a persistent volume claim that is mounted to `/var/jenkins_home`.
This assures your data is saved across crashes, restarts and upgrades.   

