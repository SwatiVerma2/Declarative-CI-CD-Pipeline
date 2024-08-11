pipeline{
    agent any
    
    stages{
        stage("Code"){
            steps{
                echo "Cloning the code"
                git url: "https://github.com/SwatiVerma2/myCICD2.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                echo "Building the code"
                sh "docker build -t my-notes-app ."
            }
        }
        stage("Push to Docker hub"){
            steps{
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker tag my-notes-app ${env.dockerHubUser}/my-note-app:v1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:v1"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "deploying the container"
                // sh "docker run -d -p 8000:8000 vswati535201/my-note-app:v1"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
