pipeline {
    agent any
    options {
        timeout(time: 3, unit: 'HOURS')
    }
    tools {
      jdk 'jdk8'
      maven 'maven-3.6.1'        
    }
    stages {
      stage('Checkout') {
        steps {
	  checkout scm
	}
      }
      stage('Build') {
        steps {
          echo "Build..."
          sh 'mvn clean package -DskipTests'
          echo "Package Successful"
        }
      }
      stage('Test') {
        steps {
          echo "Test..."
          sh 'mvn test'
          echo "Test Successful"
        }
      }
    }
}
