pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shouryat/my-node-app"
        DOCKER_REGISTRY = "docker.io"  // or Amazon ECR URL
        EC2_HOST = "43.204.220.173"
        EC2_USER = "ubuntu"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} \"
                            docker pull ${DOCKER_IMAGE}
                            docker stop my-app || true
                            docker rm my-app || true
                            docker run -d --name my-app -p 80:3000 ${DOCKER_IMAGE}
                        \"
                    """
                }
            }
        }
    }
}
