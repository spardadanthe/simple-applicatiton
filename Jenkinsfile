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
                sh '''
                #!/usr/bin/env bash
                set -x
                mvn jar:jar install:install help:evaluate -Dexpression=project.name
                set +x

                set -x
                NAME=`mvn help:evaluate -Dexpression=project.name | grep "^[^\[]"`
                set +x

                set -x
                VERSION=`mvn help:evaluate -Dexpression=project.version | grep "^[^\[]"`
                set +x

                set -x
                java -jar target/${NAME}-${VERSION}.jar

                '''
            }
        }
    }
}