pipeline {
    agent any
	stages {
	
        stage('Clone Repository'){
			steps {
				sh 'rm -rf webapps-deploy'
				sh 'git clone https://github.com/hutchhari1917/webapps-deploy'
			}
		}
		
		stage('Build Docker Image'){
			steps {
				sh 'cd /var/lib/jenkins/workspace/pipeline2/webapps-deploy'
				sh 'cp  /var/lib/jenkins/workspace/pipeline2/webapps-deploy/* /var/lib/jenkins/workspace/pipeline2'
				sh 'docker rmi hutchhari1917/pipelinetest1:v1'
				sh 'docker build -t hutchhari1917/pipelinetest1:v1 .'
			}
		}
		
	    stage('Push Image to DockerHub'){
			steps {
				sh 'docker push hutchhari1917/pipelinetest1:v1'
			}
		}
		
        stage('Deploy to Docker Host'){
			steps {
				sh 'docker -H tcp://172.31.36.111:8080 stop webapp1'
				sh 'docker -H tcp://172.31.36.111:8080 run --rm -dit --name=webapp1 --hostname=webapp1 -p 8000:80 hutchhari1917/pipelinetest1:v1'
			}
		}
		
	    stage('Check Webapp1 Rechability'){
			steps {
				sh 'sleep 10s'
				sh 'curl http://ec2-3-23-60-148.us-east-2.compute.amazonaws.com:8000'
			}
		}
	}
}
