pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = "kajolsharma/pythonflaskapp"
        //CONTAINER_NAME = "pythonflaskapp"
        
    }
    stages {
        
        stage('Build Image') {
            steps {
                withDockerRegistry([ credentialsId: "dockerhub_id", url: "https://index.docker.io/v1/" ]) {             
                //  Building new image
                sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
                sh 'docker image tag $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'

                //  Pushing Image to Repository
                sh 'docker push kajolsharma/pythonflaskapp:$BUILD_NUMBER'
                sh 'docker push kajolsharma/pythonflaskapp:latest'
                
                echo "Image built and pushed to repository"
                }
            }
           }
        stage('Deploy') {
            steps {
                sh 'docker run --name flask-app:$BUILD_NUMBER -d -p 5000:5000 $DOCKER_HUB_REPO'
            }
        }
    }
}
