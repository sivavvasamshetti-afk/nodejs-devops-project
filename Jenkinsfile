pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'kalyansagar5/nodejs-devops-app' // Docker Hub repo
    }

    stages {
        stage('GIT CHECKOUT') {
            steps {
                git branch: 'main', credentialsId: 'Kalyan_GIT_Cred', url: 'https://github.com/kalyansagar5/nodejs-devops-project.git'
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
                    docker run -d --name nodeappcontainer -p 8081:3000 $DOCKER_HUB_REPO:latest
                '''
            }
        }
    }
}
