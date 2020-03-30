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
          // sh 'mvn clean install -Dmaven.test.skip=true  -Dpmd.skip=true'
	  sh 'mvn clean package -Dmaven.test.skip=true'
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
          sh 'mvn test'
          echo "Test Successful"
	  
	  echo "P3C-PMD"
          sh "mvn pmd:pmd"
	
	  jacoco(
		execPattern: 'target/jacoco.exec',
		classPattern: 'target/classes',
		sourcePattern: 'src/main/java',
		changeBuildStatus: true,
		minimumMethodCoverage:'30',maximumMethodCoverage:'70',
		minimumClassCoverage:'30',maximumClassCoverage:'70'
		
	  )
		
	  // sh 'mvn clean'
        }
      }
    }
	
    post {
        always {
            pmd(canRunOnFailed: true, pattern: '**/target/pmd.xml')
	    // junit testResults: "**/target/surefire-reports/*.xml"
        }
	failure {
	    mail from : 'jenkinsx@163.com', 
		 to : '1989153584@qq.com',
	         subject : 'Failed Pipeline: ${currentBuild.fullDisplayName} :(', 
	         body : 'Something is wrong with ${env.BUILD_URL}'
	}
    }
}
