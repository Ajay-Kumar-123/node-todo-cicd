pipeline {
    agent { label "dev" }
    stages {
        stage("Pull Code"){
            steps{
                git url: "https://github.com/Ajay-Kumar-123/node-todo-cicd.git", branch: "master"
                echo "Pulling code from Github"
            }
        }
        
        stage("Build and Test Stage"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo "Building and Testing the application" 
            }
        }
        
        stage("Scan Image"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                echo "Scan the Docker image for vulnerabilities"
                sh "trivy image ${env.dockerhubUser}/node-app-test-new:latest --scanners vuln"
                }
            }
        }
        
        stage("Push latest Docker image to Dockerhub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerhubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerhubUser}/node-app-test-new:latest"
                echo "Pushed Docker image to Dockerhub"
                }
            }
        }
        
        stage("Deploy the application"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo "Deployed the application"
            }
        }
    }
}
