pipeline {
    environment {
        IMAGEN = "donrich4r/pin1-grupo7"
        USUARIO = 'USER_DOCKERHUB'
    }
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: "main", url: 'https://github.com/ricardofcarrion/pin1-group7.git'
            }
        }
        stage('Build') {
            steps {
                script {
                    newApp = docker.build "$IMAGEN:$BUILD_NUMBER"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("$IMAGEN:$BUILD_NUMBER").inside('-u root') {
                           sh 'nginx -v'
                        }
                    }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry( '', USUARIO ) {
                        newApp.push()
                    }
                }
            }
        }
        stage('Clean Up') {
            steps {
                sh "docker rmi $IMAGEN:$BUILD_NUMBER"
                }
        }
    }
}
