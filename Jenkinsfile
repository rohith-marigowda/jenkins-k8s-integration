pipeline {
    agent any
    environment {
	AWS_REGION = "ap-south-1"
	DOCKER_REPO_NAME = "rohithmarigowda/assignment"
	DOCKER_IMAGE_MASTER = "$DOCKER_REPO_NAME:${BRANCH_NAME}"
	DOCKER_IMAGE_LATEST = "$DOCKER_REPO_NAME:latest"
}
    stages {
        stage('Git checkout') {
            steps {
                echo 'We are now checking out the git repository'
                git 'https://github.com/rohith-marigowda/javaProject.git'
            }
        }

	 stage('Run Unit and Integration tests'){
  	    steps {
		echo 'In this step run unit and integration tests'
	    }
	 }

	stage('SonarQube analysis') {
        steps{ 
        	sh 'echo execute below command for sonar code analysis'
		//sh "mvn sonar:sonar"
        }
        }
	    
         stage('Build docker image') {
            steps {
		    echo 'test'
		sh 'docker build --tag ${DOCKER_IMAGE_MASTER} .'
		sh 'docker build --tag ${DOCKER_IMAGE_LATEST} .'
            }	 
        }
	    
        stage('docker image push') {
	   steps {
        	sh 'docker login -u $docker_username -p $docker_password'
		sh 'docker push ${DOCKER_IMAGE_MASTER}'
		sh 'docker push ${DOCKER_IMAGE_LATEST}'
    	}
}
	    //stage('Deploy to k8s'){
            //steps{
                //script{
                    //kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                //}
            //}
        //}
    }
}
