pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout - Go to github(*Dockerfile)') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/K4SUND/CICD-jenkins-docker'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t nodeapp:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                
                withCredentials([string(credentialsId: 'docker-hub-p', variable: 'password')]) {
                    // some block
                     script {
                        bat "docker login -u k4sund -p %password%"
                    }
                    }
                
                
                // withCredentials([string(credentialsId: 'samin-docker', variable: 'samindocker')]) {
                //     script {
                //         bat "docker login -u adomicarts -p %samindocker%"
                //     }
                // }
            }
        }
        stage('Push Image to dockerHub') {
            steps {
                bat 'docker push nodeapp:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
