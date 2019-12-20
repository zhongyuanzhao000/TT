pipeline {
    agent any
    tools {
      jdk 'jdk8'
      maven 'maven-3.6.1'        
    }
    stages {
      stage('Build') {
        steps {
          echo "Build..."
          sh 'mvn clean package -DskipTests'
          echo "Package Successful"
	  sh 'pwd'
	  sh 'source /etc/profile'
	  sh 'sudo docker login -u codewisdom -p "$DOCKERHUB_PWD"'
	  sh 'sudo docker push codewisdom/codewisdom/hello-world:latest'
        }
      }
      stage('Test') {
        steps {
          echo "Test..."
          sh 'mvn test'
          echo "Test Successful"
	  sh 'mvn clean'
        }
      }
    }
}
