pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'sivav2516/nodejs-devops-app' // Docker Hub repo
    }

    stages {
        stage('GIT CHECKOUT') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'SHIVACRED', url: 'https://github.com/sivavvasamshetti-afk/nodejs-devops-project.git']])
            }
        }

        stage('DEPENDENCY') {
            steps {
                dir('app') {
                    sh 'npm install'
                }
            }
        }

        stage('BUILD DOCKER') {
            steps {
                sh 'docker build -t nodeapp .'
            }
        }

        stage('DOCKER LOGIN & PUSH') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Kalyan_Docker_Cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker tag nodeapp $DOCKER_HUB_REPO:latest
                        docker push $DOCKER_HUB_REPO:latest
                    '''
                }
            }
        }

        stage('RUN CONTAINER') {
            steps {
                sh '''
                    docker stop nodeappcontainer || true
                    docker rm nodeappcontainer || true
                    docker run -d --name nodeappcontainer -p 8083:3000 $DOCKER_HUB_REPO:latest
                '''
            }
        }
    }
}
