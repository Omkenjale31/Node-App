pipeline{
    agent any
    stages{
        stage("Build"){
             step{
                echo "Start Building"
                sh "npm install"
                sh "npm run build"
                echo "Building Completed"
            }
        }
       
    }
}