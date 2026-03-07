pipeline {
    agent any

    stages {
        stage('GIT CHECKOUT') {
            steps {
                git branch: 'main', credentialsId: 'Kalyan_GIT_Cred', url: 'https://github.com/kalyansagar5/nodejs-devops-project.git'
            }
        }
        stage('DEPENDENCY') {
            steps {
               sh 'npm install'
            }
        }
        stage('BUILD DOCKER') {
            steps {
               sh 'docker build -t nodeapp .'
            }
        }
        stage('RUN CONTAINER') {
            steps {
                sh '''
                docker stop nodeappcontainer || true
                docker rm nodeappcontainer || true
                docker run -d --name nodeappcontainer -p 8081:8080 nodeapp
                '''
            }
        }
        
    }
}

