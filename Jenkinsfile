pipeline {
    agent any
    environment {
        imagename = "kajolsharma/pythonflaskapp"
        registryCredential='dockerhub_id'
        dockerImage  = "flask-container"
         }
	stages{   
        stage('Build') {
            steps {
                               
                /*  Building new image
                sh 'docker image build -t $DOCKER_HUB_REPO:latest .'
                sh 'docker image tag $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'

                //  Pushing Image to Repository
                sh 'docker push kajolsharma/pythonflaskapp:$BUILD_NUMBER'
                sh 'docker push $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'
                
                echo "Image built and pushed to repository"
                */
               
                script {
		dockerImage = docker.build imagename
		}
}
}
stage('Deploy Image') {
steps{
script {
docker.withRegistry( '', registryCredential ) {
dockerImage.push("$BUILD_NUMBER")
dockerImage.push('latest')
}
}
}
}
        stage('Deploy') {
            steps {
                script{
                    //sh 'BUILD_NUMBER = ${BUILD_NUMBER}'
                    if (BUILD_NUMBER == "1") {
                        sh 'docker run --name $dockerImage -d -p 5000:5000 $imagename'
                    }
                    else {
                        sh 'docker stop $dockerImage'
                        sh 'docker rm $dockerImage'
                        sh 'docker run --name $dockerImage -d -p 5000:5000 $imagename'
                    }
                    //sh 'echo "Latest image/code deployed"'
                }
            }
        }
    }
}
