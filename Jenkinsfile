def customImage = '';
def oldImages = '';


pipeline {
    agent any
    options { timestamps() }

    environment {
        AWS_ACCOUNT_ID="122483003428"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="cognizantadvanced"
        IMAGE_TAG="${env.BUILD_ID}"
        REPOSITORY_URI = "https://${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }

    ##CICD Updated
    
    stages {
        
        stage('Building image') {
            steps{
                script {
                    customImage = docker.build("${IMAGE_REPO_NAME}:${IMAGE_TAG}")
                }
            }
        }
   
        stage('Pushing to ECR') {
            steps{  
                script {
                    docker.withRegistry("${REPOSITORY_URI}") {
                        customImage.push()
                        customImage.push("latest")
                    }
                }
            }
        }
    }
    
    post {
        always {
            script {
                oldImages = sh(script: "docker images -q ${IMAGE_REPO_NAME}:${IMAGE_TAG}", returnStdout: true)
                sh "docker rmi -f ${oldImages}"
            }
            cleanWs()
        }
    }
}
