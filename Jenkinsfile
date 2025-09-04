@Library('jenkins-shared-libraries')_

def configMap = [
    project: "roboshop",
    component: "catalogue"
]
if(! env.BRANCH_NAME.equalsIgnoreCase('main')){
    nodejsEKSPipeline(configMap)
}
else{
    echo "Please proceed with prod"
}