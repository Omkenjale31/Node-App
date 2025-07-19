pipeline{
    agent any

    environment {
        EC2_HOST ='13.233.137.32'
        SSH_CREDENTIAL_ID = 'omkenjale'
        REMOTE_USER = 'ubuntu'
        REMOTE_PATH = '/home/ubuntu/app'
        WEB_ROOT = '/var/www/html'
        
    }
    stages{
        stage('Build'){
             steps{
                echo "Installing dependencies.."
                sh "npm install"

                echo 'Building the application...'
                sh "npm run build"
                echo "Building Completed."
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying  to EC2 at ' + env.EC2_HOST
                sshagent (credentials: [env.SSH_CREDENTIAL_ID]) 

                sh '''
                    echo "Creating remote directory..."
                    ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} 'mkdir -p ${REMOTE_PATH}'

                    echo "Copying build to re,ote EC2.."
                    scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/
                    echo "Moving file to web root and restarting nginx..."
                    ssh ${REMOTE_USER}@${EC2_HOST} '

                     sudo rm -rf ${WEB_ROOT}/*
                     sudo cp -r ${REMOTE_PATH}/* ${WEB_ROOT}/
                     sudo systemctl restart neinx
                    '

                    echo "Restarting $SERVICE_NAME"
                    sudo systemctl restart ${SERVICE_NAME}
                '''

                echo 'Deployment completed.'    
            }
        }
       
    }
}