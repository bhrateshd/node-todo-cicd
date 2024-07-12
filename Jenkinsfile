pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/bhrateshd/node-todo-cicd.git", branch: "master"
                echo 'Code clone completed'
            }
        }
        
        stage("Build & Test") {
            steps {
                sh "docker build -t node-app-test-new ."
                echo 'Code build completed'
            }
        }
        
        stage("Scan image") {
            steps {
                echo 'Image scanning completed'
                // Integrate actual security scanning tools here if needed
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")])
                {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'Code push completed'
                // Implement DockerHub push steps here
                
                }
            }
        }
        
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo 'Code deploy completed'
            }
        }
    }
}
