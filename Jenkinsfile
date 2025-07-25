pipeline {
    agent any

    tools {
        maven 'MAVEN_HOME' // Make sure Maven is configured in Jenkins
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/NiranjithaGaneshan/spring-petclinic.git'
            }
        }

        stage('Build with Maven') {
            steps {
               bat 'mvn clean package -DskipTests'
            }
        }

        stage('Archive JAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t petclinic-app .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                    docker stop petclinic-container || true
                    docker rm petclinic-container || true
                    docker run -d -p 8085:8080 --name petclinic-container petclinic-app
                '''
            }
        }
    }
}
