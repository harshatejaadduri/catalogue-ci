# Continous Integration
It is a Developers repo and in this repo continous intergration for components will take place and things like new versions and bug fixes will checkout here and code will be cloned by devops team and do the further proceedings like unit testing, functional testing etc and if any thing goes wrong they report to concern Developers.

* In this case of CI Code
![screenshot1](catalogue-ci\prebuild.png)

we are creating a prebuild job which includes environmental variables, agent which is connecting and some options

![screenshot2](catalogue-ci\reading package.png)

here we are updating code version and giving it to appVerison which helps to build docker image with that version

![screenshot3](catalogue-ci\install dependencies.png)

here we are installing code dependencies

![screenshot4](catalogue-ci\pushing to ecr.png)

after building docker image sourcing version from the code we are pushing it to ECR 


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
