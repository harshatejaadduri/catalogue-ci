pipeline{
    agent{
        label 'AGENT-1'
    }
    environment{
        ACC_ID ='513993748676'
        appVersion = ''
        REGION = 'us-east-1'
        PROJECT = 'roboshop' 
        COMPONENT = 'catalogue'
    }   
    options{
        disableConcurrentBuilds()
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
                    expression{params.deploy_to == 'dev'}
                    }
            steps {
                script{
                    build job: 'catalogue-cd', 
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