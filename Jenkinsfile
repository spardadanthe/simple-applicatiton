pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Building Project') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Running tests') {
            steps {
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
                sh 'bash ./jenkins/scripts/deliver.sh' 
            }
        }
    }
}