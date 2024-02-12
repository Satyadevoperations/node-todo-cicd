pipeline {
    agent { label 'dev-agent' }
    
    stages{
        stage('code'){
           steps {
               git url: 'https://github.com/Satyadevoperations/node-todo-cicd.git', branch: 'master'
           }
        }
        stage('Build and Test'){
           steps {
               sh 'docker build . -t narayanhari/node-todo-app-cicd:latest'
			   sh 'docker run -d -p 8080:8080 narayanhari/node-todo-app-cicd:latest'
           }
        }
        stage('Login and Push Image'){
           steps {               
               withCredentials([usernamePassword(credentialsId:'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerhubUser')]) {
                   sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
				   sh "docker tag node-todo-app:latest narayanhari/node-todo-app-cicd:latest"
                   sh "docker push narayanhari/node-todo-app-cicd:latest"
				   echo 'logging into docker hub and pushing image'
               }
           }
        }
        stage('Deploy'){
           steps {
               sh 'docker-compose down && docker-compose up --d'
           }
        }
    }
}
