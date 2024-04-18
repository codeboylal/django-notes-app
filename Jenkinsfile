pipeline {
    agent any
    
    stages {
        stage ('Fetch Code') {
            steps{
                echo 'Cloning code from github'
                git url:'https://github.com/codeboylal/django-notes-app.git', branch: 'main'
            }
        }
        
        stage ('Build') {
            steps {
                echo 'Building the image'
                sh 'docker build -t my-notes-app .'
            }
        }
        
        // Alternative way of adding and using credentials

        // stage('Push to DockerHub') {
        //     steps {
        //         echo 'Pushing image to dockerhub'
        //         withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'dockerhubUser', passwordVariable: 'dockerhubPass')]) {
        //             sh "docker login -u ${env.dockerhubUser} -p ${dockerhubPass}"
        //         }
        //     }
        // }

      // Convienent way of adding and using docerhub credentials
        stage ('Push to DockerHub') {
            steps {
                echo 'Pushing image to dockerhub'
                    withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'dockerhubUser', passwordVariable: 'dockerhubPass')]) {
                    sh "docker tag my-notes-app $dockerhubUser/my-notes-app:latest"
                    sh "docker login -u $dockerhubUser -p $dockerhubPass"
                    sh "docker push $dockerhubUser/my-notes-app:latest"
                }
            }
        }

        // This is code block is recommended for deploying production-like environment - First it down and remove all containers and build the clean new one
        stage ('Deploy') {
            steps{
                echo 'Deploying to container'
                //sh 'docker run -d -p 8000:8000 lalbudha47/my-notes-app:latest'
                //sh 'docker-compose up -d'- while in Dev or Testing env we can keep all the containers 
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
