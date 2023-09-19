pipeline{
    agent none

    environment{
        BUILD_SERVER_IP= 'ec2-user@13.232.190.231'
        DEPLOY_SERVER_IP= 'ec2-user@3.109.58.101'
        IMAGE_NAME = "mudassir12/micro-service-app:php${BUILD_NUMBER}"

        
    }

    stages{
        stage('BUILD THE DOCKERFILE AND PUSH TO THE DOCKERHUB'){
            agent any

            steps{
                script{
                    sshagent(['build-server-key']){

                        //withCredentials is used to bind your Docker Hub Credentials with the variables so that your credentials is encrypted. 
                        withCredentials([usernamePassword(credentialsId: 'DockerHubCredentials', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){

                        
                        echo "Packaging the apps"

                        sh "scp -o StrictHostKeyChecking=no -r Docker-files ${BUILD_SERVER_IP}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${BUILD_SERVER_IP} 'bash /home/ec2-user/Docker-files/docker-script.sh'
                        sh "ssh ${BUILD_SERVER_IP} sudo docker build -t ${IMAGE_NAME} -f /home/ec2-user/Microservice_PHP/Docker-files/dockerfile ."
                        sh "ssh ${BUILD_SERVER_IP} sudo docker login -u ${USERNAME} -p ${PASSWORD}"
                        sh "ssh ${BUILD_SERVER_IP} sudo docker push ${IMAGE_NAME}"
                        }
                    }
                }
            }
        }
    }
}