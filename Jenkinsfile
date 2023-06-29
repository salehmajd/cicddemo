pipeline {
    agent any

    environment {
        mavenhome = tool 'jenkins-maven'
        imageName = "cicddemo"
        registryCredentials = "nexus"
        registry = ""
        dockerImage = "http://localhost:9001/repository/nexus-docker/"
    }

    tools {
        jdk 'jenkins-jdk'
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

        stage('Build image') {
                    steps {
                        script {
                            dockerImage = docker.build imageName
                        }
                    }
                }

        stage('Uploading to Nexus') {
             steps{
                 script {
                     docker.withRegistry(registry, registryCredentials ) {
                     dockerImage.push('latest')
                  }
                }
              }
            }

            // Stopping Docker containers for cleaner Docker run
            stage('stop previous containers') {
                 steps {
                    sh 'docker ps -f name=cicddemo -q | xargs --no-run-if-empty docker container stop'
                    sh 'docker container ls -a -fname=cicddemo -q | xargs -r docker container rm'
                 }
               }

            stage('Docker Run') {
               steps{
                 script {
                        sh 'docker run -d -p 8080:8080 --rm --name cicddemo' + registry + imageName
                    }
                 }
              }
            }
        }
}