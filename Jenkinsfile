pipeline {
    agent any
    stages{
        stage ("Code Cloned"){
            steps{
                git url: "https://github.com/SanketShirke/django-todo-app.git", branch: "main"
                echo "Code Cloned"
            }
        }
        stage ("Code Build"){
            steps{
                sh "docker build . -t django-app-demo"
                echo "Code Build"
            }
        }
        stage ("Push the image to Docker Hub"){
            steps {
            withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
            sh "docker tag django-app-demo ${env.dockerHubUser}/django-app-demo:latest"
            sh "docker push ${env.dockerHubUser}/django-app-demo:latest"
                }
                echo "Image push to Docker Hub"
            }
        }
        stage ("Deploy the Application on EC2 Instance"){
            steps {
                sh "docker-compose down && docker-compose up -d"
                echo "Code has been deployed on EC2"
            }
        }
    }
}
