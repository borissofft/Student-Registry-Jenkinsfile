pipeline {
    agent any

    stages {
        stage("Repo checkout") {
            steps {
                checkout scm
            }
        }

        stage('NPM Install') {
            steps {
                bat "npm install"
            }
        }

        stage('Run integration tests') {
            steps {
                bat "npm run test"
            }
        }

        stage('Build Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: '5ae6209d-a5e5-441d-91aa-f02289f112c1', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat """docker build -t kolegata86/student:1.0.0 .
                        docker login -u %DOCKER_USERNAME% --password %DOCKER_PASSWORD%
                        docker push kolegata86/student:1.0.0"""
                }
            }
        }
        
        stage('Deploy Image') {
            steps {
              script {
                    // Prompt for input approval
                    input("Deploy to production?") 
                }
                withCredentials([usernamePassword(credentialsId: '5ae6209d-a5e5-441d-91aa-f02289f112c1', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    bat """docker pull kolegata86/student:1.0.0
                    docker run -d -p 8080:8080 kolegata86/student:1.0.0 """
                }
            }
        }

    }
}