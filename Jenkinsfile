pipeline {
    agent any
    tools{
        jdk 'java-11'
        maven 'maven'
    }
    stages{
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Rachana-js/test-1.git'
            }
        }
        stage('compile'){
            steps{
                sh "mvn compile"
            }
        }
        stage('Build'){
            steps{
                sh "mvn clean install" 
            }
        }
        stage('Build and Tag Docker file'){
            steps{
                sh "docker build -t rachanajs/demonew:1 ."
            }
        }
        stage('Docker image scan'){
            steps{
                 sh "trivy image --format table -o trivy-image-report.html rachanajs/demonew:1"
            }
        }

        stage('Containersation'){
            steps{
                sh '''
                    docker stop c3
                    docker rm c3
                    docker run -it -d --name c3 -p 9003:8080 rachanajs/demonew:1
                '''
            }
        }

        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                        }
                    }
        }

        stage('Pushing image to repository'){
            steps{
                sh 'docker push rachanajs/demonew:1'
            }
        }


    }
}