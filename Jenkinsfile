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
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                    stash(name: 'app', includes: 'target/**')
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                unstash 'app'
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Confirm Deploy'){
            when {
                branch 'master'
            }
            steps{
                input(message: 'Okay to Deploy?', ok: 'Yes, let\'s do it!')
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                unstash 'app'
                sh 'chmod +x scripts/deploy.sh'
                sh './scripts/deploy.sh'
            }
        }
    }
}
