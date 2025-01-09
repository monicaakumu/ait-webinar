pipeline {
    agent any
    tools {
        maven 'Maven3'
    }
    stages {
        stage('Hello Africa') {
            steps {
                echo 'My First Declarative Script'
            }
        }
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/monicaakumu/ekangaki-maven-web-app.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t moniakumu/maven-web-application:${BUILD_NUMBER} .'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker run -it -d -p 3035:8080 moniakumu/maven-web-application:${BUILD_NUMBER}'
            }
        }
        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials-id', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh '''
                            docker login -u ${DOCKER_USER} -p ${DOCKER_PASS}
                            docker push moniakumu/maven-web-application:${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline execution completed.'
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed.'
        }
    }
}
