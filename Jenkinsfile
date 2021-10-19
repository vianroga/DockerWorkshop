pipeline {
    
    agent any
    
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub-pass')
    }
    
    stages {
        stage('Prepared') {
            steps {
                echo 'Clonando repo en jenkins'
                git branch: "master" , url:'https://github.com/vianroga/DockerWorkshop/'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Contruyendo imagen docker desde el Dockerfile del repo'
                sh "docker build -t victor21/minecraft:'${env.BUILD_ID}' ."
            }
        }
    
    
        stage('Analysis') {
            steps {
                echo 'Analizando imagen construida'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh "docker scan --accept-license --severity=medium victor21/minecraft:'${env.BUILD_ID}'"
            }
        }
    
    
        stage('Push') {
            steps {
                echo 'Enviando imagen a docker hub'
                sh "docker push victor21/minecraft:'${env.BUILD_ID}' "
                sh 'docker logout' 
            }
        }
    
    
        stage('Deploy') {
            steps {
                echo 'Desplegando el contenedor en local'
                sh 'docker rm -f minicraft-server echo "Desplegando docker image" || echo "Desplegando docker image"'
                sh "docker run -d --name minicraft-server -p 25565:25565 victor21/minecraft:'${env.BUILD_ID}'"
            }  
        }
    }
}
