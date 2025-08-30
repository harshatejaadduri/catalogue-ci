pipeline{
    agent{
        label 'AGENT-1'
    }
    environment{
        ACC_ID = "513993748676"
        appVersion = ''
        REGION = "us-east-1"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }   
    options{
        disableConcurrentBuilds()
    }
    parameters{
        booleanParam(name: 'deploy', defaultValue: false, description: 'Toggle this value')
    }
    stages{
        stage('Read JSON'){
            steps{
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "Package version: ${appVersion}"
                }
            }
        }
        stage('Install dependencies'){
            steps{
                script{
                    sh """
                        npm install
                        """
                }
            }
        }
        stage('Sonar Scan') {
            environment {
                scannerHome = tool 'sonar-7.2'
            }
            steps {
                script {
                   // Sonar Server envrionment
                   withSonarQubeEnv(installationName: 'sonar-7.2') {
                         sh "${scannerHome}/bin/sonar-scanner"
                   }
                }
            }
        }
        stage('Docker Build'){
            steps{
                script{
                    withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                        sh """
                            aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
                            docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
                            docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
                            """
                    }      
                }
            }
        }
        stage('Trigger Deploy'){
            when{
                    expression{ params.deploy }
                    }
            steps {
                script{
                    build job: 'catalogue-cd',
                    parameters: [
                        string(name: 'appVersion', value: "${appVersion}"),
                        string(name: 'deploy_to', value: 'dev')
                          ],
                    wait: false
                }
            }
       }
    }
    post{
        always{
            echo 'Hello World'
            deleteDir()
        }
       success{
        echo 'Code Success'
       } 
       failure{
        echo 'Code Failed'
       }
    }   
}