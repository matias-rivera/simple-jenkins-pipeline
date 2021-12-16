pipeline {
    agent any
    tools {
        maven 'maven-3.8.4'
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                echo 'Deploying...'
                sh 'chmod +x scripts/deploy.sh'
                sh './scripts/deploy.sh'
            }
        }
    }
}
