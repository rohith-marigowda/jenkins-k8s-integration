pipeline {
    agent any
    environment {
	AWS_REGION = "ap-south-1"
	AWS_ACCESS_KEY = credentials('aws_accesskey_id')
	AWS_SECRET_KEY = credentials('aws_secretkey_id')
	AWS_ECR_URL = "878226295837.dkr.ecr.ap-south-1.amazonaws.com"
	AWS_ECR_REPONAME = "$AWS_ECR_URL/dockerassignment-cicd"
	DOCKER_IMAGE_MASTER = "$AWS_ECR_REPONAME:${BRANCH_NAME}"
	DOCKER_IMAGE_LATEST = "$AWS_ECR_REPONAME:latest"
	
	ECR_LOGIN = "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin ${AWS_ECR_URL}"
}
    stages {
        stage('Git checkout') {
            steps {
                echo 'We are now checking out the git repository'
		echo '$AWS_ACCESS_KEY'
                git 'https://github.com/rohith-marigowda/javaProject.git'
            }
        }

	 stage('Run Unit and Integration tests'){
  	    steps {
		echo 'In this step run unit and integration tests'
	    }
	 }
	    
         stage('Build docker image') {
            steps {
		    echo 'test'
		sh 'docker build --tag ${DOCKER_IMAGE_MASTER} .'
		sh 'docker build --tag ${DOCKER_IMAGE_LATEST} .'
            }	 
        }
	    
        stage('docker image push to ECR') {
    environment {
        AWS_ACCESS_KEY = credentials('aws_accesskey_id')
        AWS_SECRET_KEY = credentials('aws_secretkey_id')
    }

    steps {
        script {
            sh """
                export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY}
                export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_KEY}
                aws configure set aws_access_key_id ${AWS_ACCESS_KEY}
                aws configure set aws_secret_access_key ${AWS_SECRET_KEY}
                aws configure set default.region ${AWS_REGION}
                aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ECR_URL}
                docker push ${DOCKER_IMAGE_MASTER}
                docker push ${DOCKER_IMAGE_LATEST}
            """
        }
    }
}
	    stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
