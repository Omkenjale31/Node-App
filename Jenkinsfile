pipeline{
    agent any

    environment {
        DEPLOY_DIR = 'var/www/html'
        SERVICE_NAME ='nginx'
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
                echo 'Deploying build to local web server...'

                sh '''
                    echo "Copying build to $DEPLOY_DIR"
                    sudo rm -rf ${DEPLOY_DIR}/*
                    sudo cp -r dist/* ${DEPLOY_DIR}/

                    echo "Restarting $SERVICE_NAME"
                    sudo systemctl restart ${SERVICE_NAME}
                '''

                echo 'Deployment completed.'    
            }
        }
       
    }
}