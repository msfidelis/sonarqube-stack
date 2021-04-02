# Running Local

## Setup local stack

```
docker-compose up -d postgresql
docker-compose up --force-recreate
```

## Login 

Access http://0.0.0.0:9000 in your browser and wait for sonar setup 

The initial user and password for admin user is `admin/admin`. Change on first access. 

## Generate your user token for CI 

Access in your user menu, `My Account` > `Security` > `Generate Tokens`

![account](/.github/img/account.png)

![token](/.github/img/token.png)

## Configure sonar-project.properties on your application 

```bash
echo '''
sonar.projectKey=chip
sonar.projectName=chip
sonar.sources=./
sonar.sourceEncoding=UTF-8
''' > sonar-project.properties
```

## Run your first scan

```
docker run \
    --rm \
    -e SONAR_HOST_URL="http://0.0.0.0:9000" \
    -e SONAR_LOGIN=2f165ff1a9ed5241471d0a9ce32e8e93ac63ffa5 \
    -v "$(pwd):/usr/src" \
    --network host \
    sonarsource/sonar-scanner-cli
```

# Kubernetes 

### Create namespace 

```bash 
kubectl apply -f namespaces.yml
```

### Edit and create configmaps and secrets 

```bash 
kubectl apply -f configmap.yml
```

### Deploy a postgresql stack

```bash 
kubectl apply -f postgresql.yml
```

### Deploy a sonarqube stack

```bash 
kubectl apply -f sonarqube.yml
```

## Run your first scan

```
docker run \
    --rm \
    -e SONAR_HOST_URL="http://sonarqube.raj.ninja" \
    -e SONAR_LOGIN=2f165ff1a9ed5241471d0a9ce32e8e93ac63ffa5 \
    -v "$(pwd):/usr/src" \
    --network host \
    sonarsource/sonar-scanner-cli
```

