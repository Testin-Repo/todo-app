pipeline {
    agent any

    stages {
        stage("Clone") {
            steps {
                echo "Cloning the code"
                git url:"https://github.com/Testin-Repo/todo-app.git", branch:"main"
            }
        }
        stage("Build") {
            steps {
                echo "Build the image"
                sh "docker build -t my-image ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerPass", usernameVariable:"dockerUser")]){
                sh "docker tag my-image ${env.dockerUser}/my-image:latest"
                sh "docker login -u ${env.dockerUser} -p ${env.dockerPass}"
                sh "docker push ${env.dockerUser}/my-image:latest"
                    
                }
            }
        }
        stage("Depoly") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
