pipeline {
    agent any

    environment {
        EC2_HOST = "43.204.228.38"
        SSH_CREDENTIAL_ID = "ec2-key"
        REMOTE_USER = "ubuntu"
        REMOTE_PATH = "/home/ubuntu/app"
        WEB_ROOT = "/var/www/html"
        SERVER = "nginx"
    }

    stages {
        stage("Build") {
            steps {
                echo "Installing Dependencies"
                sh "npm install"

                echo "Building the app"
                sh "npm run build"

                echo "Build complete"
            }
        }

        stage("Deploy") {
            steps {
                echo "Starting Deployment"

                sshagent(credentials: [env.SSH_CREDENTIAL_ID]) {
                    sh """
                        echo "Creating remote directory"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} 'mkdir -p ${REMOTE_PATH}'

                        echo "Copying distribution files to remote path"
                        scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/

                        echo "Restarting Nginx and deploying files"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} '
                            sudo rm -rf ${WEB_ROOT}/*
                            sudo cp -r ${REMOTE_PATH}/* ${WEB_ROOT}/
                            sudo systemctl restart ${SERVER}
                        '
                    """
                }
            }
        }
    }
}
