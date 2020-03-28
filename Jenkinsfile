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
          sh 'mvn clean install -Dmaven.test.skip=true  -Dpmd.skip=true'
          echo "Package Successful"
		
	  // sh 'sudo /usr/local/bin/docker-compose build'
		
          script {
            def currentBranch =  env.BRANCH_NAME
	    if (currentBranch == 'master') {
	      echo "This is master branch. Skip push step."
	    
	    } else {
	      withCredentials([usernamePassword(credentialsId: 'dockerHub-codewisdom', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
	        sh "sudo docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
	        // sh "sudo /bin/bash ./image-tag-push.sh"
	      }
	      echo "Push Successful"
	    }
	  
	  }
	  // sh 'sudo /bin/bash ./clean.sh'
        }
      }
      stage('Test') {
        steps {
          echo "Test..."
          //sh 'mvn test'
          echo "Test Successful"
	  
	  echo "P3C-PMD"
          sh "mvn pmd:pmd"
		
	  // sh 'mvn clean'
        }
      }
    }
	
    post {
        always {
            pmd(canRunOnFailed: true, pattern: '**/target/pmd.xml')
        }
	failure {
	    mail subject : 'The Pipeline failed :(', body : 'failure body', from : 'jenkinsx@163.com', to : '1989153584@qq.com'
	}
    }
}
