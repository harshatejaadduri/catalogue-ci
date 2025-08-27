# Continous Integration
It is a Developers repo and in this repo continous intergration for components will take place and things like new versions and bug fixes will checkout here and code will be cloned by devops team and do the further proceedings like unit testing, functional testing etc and if any thing goes wrong they report to concern Developers.

* In this case of CI Code
### 1. Prebuild
![Prebuild Job](https://github.com/user-attachments/assets/46f2d1b3-b753-4cfe-81e4-d5647313e629)

we are creating a prebuild job which includes environmental variables, agent which is connecting and some options

### 2. Reading Package Version
![Reading Package Version](https://github.com/user-attachments/assets/57707eeb-0505-474c-b793-e58c6f9bbb9a)

here we are updating code version and giving it to appVerison which helps to build docker image with that version

### 3. Installing Dependencies
![Installing Dependencies](https://github.com/user-attachments/assets/2eddcbd5-7681-4a35-82a9-ebe90e027ee2)

here we are installing code dependencies

### 4. Pushing to ECR
![Pushing to ECR](https://github.com/user-attachments/assets/24fcc060-b479-4c1b-94ec-1f88b04a6273)

after building docker image sourcing version from the code we are pushing it to ECR 



## CI/CD Pipeline

## Push Commands for ECR
### Assign Variables
```
ACC_ID=513993748676
PROJECT=roboshop
appVersion=''
REGION=us-east-1

```
aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
```

## Docker Build

```
docker build -t 513993748676.dkr.ecr.us-east-1.amazonaws.com/roboshop/catalogue:latest
```
```
docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
```


## Docker Push

```
docker push 513993748676.dkr.ecr.us-east-1.amazonaws.com/roboshop/catalogue:latest
```
```
docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
```
