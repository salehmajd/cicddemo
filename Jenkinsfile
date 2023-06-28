pipeline {
    agent any

    environment {
        mavenhome = tool 'jenkins-maven'
    }

    tools {
        jdk 'jenkins-java'
    }

    stages {
        stage('Build') {
            steps {
                bat "mvn clean -DskipTests install"
            }
        }

        stage('Test') {
            steps {
                bat "mvn test"
            }
        }

        stage('Deploy') {
            steps {
                bat "mvn jar:jar deploy:deploy"
            }
        }
    }
}