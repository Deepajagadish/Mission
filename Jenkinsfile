pipeline {
    agent any 
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    environment {
      SCANNER_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('Cleaning Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Deepajagadish/Mission.git'
            }
        }
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('Test') {
            steps {
                sh "mvn test -DskipTests=true"
            }
        }
        stage('Trivy scan') {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("SonarQube Analysis"){
           steps {
		          withSonarQubeEnv('sonar') { 
                    sh ''' $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=Mission \
                        -Dsonar.projectKey=Mission \
                        -Dsonar.java.binaries=. '''
		            }
	            }	
           }
           stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }
       
   }
}
