pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/NiranjithaGaneshan/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Docker Build & Run') {
            steps {
                sh '''
                    docker build -t petclinic-app .
                    docker run -d -p 8085:8080 --name petclinic-container petclinic-app
                '''
            }
        }
    }
}
