pipeline {
    agent any

    environment {
        DOCKER_USER = "sivav2516"
        IMAGE_NAME = "take_it"
        IMAGE_TAG = "0.1"
    }

    stages {

        stage('GIT CHECKOUT') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'SHIVACRED',
                        url: 'https://github.com/sivavvasamshetti-afk/nodejs-devops-project.git'
                    ]]
                )
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

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKER_CRED', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo "$PASS" | docker login -u "$USER" --password-stdin
                    docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    docker push ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f take_it || true'
                sh 'docker run -d -p 5121:8080 --name take_it ${DOCKER_USER}/${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }

    }
}
