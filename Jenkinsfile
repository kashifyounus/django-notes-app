pipeline {
    agent {label "jenkinnode"}

    stages {
        stage('Clone') {
            steps {
                echo 'Cloning Project'
                git url: "https://github.com/kashifyounus/django-notes-app", branch:"main"
            }
        }
         stage('Build') {
            steps {
                echo 'Building Project'
                //sh "whoami"
                sh "docker build -t notes-app:latest ."
                
                
            }
        }
        stage('Test') {
            steps {
               echo 'Testing Project'
            }
        }
        stage('Push to DockerHub') {
            steps {
                 echo 'Pushing Docker image to Docker Hub'
                 ///withCredentials([usernamePassword(credentialsId:"dockerHubCreds",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){

                 withCredentials([usernamePassword(
                     credentialsId: "dockerHubCreds", 
                     passwordVariable:'dockerHubPass', 
                     usernameVariable: 'dockerHubUser')])
                     {
                         sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                         sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                         sh "docker push ${env.dockerHubUser}/notes-app:latest"
                     }
            }
        }
        stage('Deploy') {
            steps {
                 echo 'Deploying Project'
                 sh "docker compose up -d"
                 //sh "docker run -d -p 8000:8000 notes-app:latest"
            }
        }
    }
}
