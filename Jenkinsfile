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
	  withCredentials([usernamePassword(credentialsId: 'dockerHub-codewisdom', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
	    sh "sudo docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
	    sh "sudo docker push codewisdom/hello-world:latest"
	  }
	  echo "Push Successful"
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
