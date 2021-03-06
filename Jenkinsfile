pipeline {
    agent any
    
    tools {
        maven 'Maven 3.6.3'
        
    }
    
    stages {
        
        stage('Checkout') {
            steps {
                echo 'Entered inside my checkout'
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/anandsk26/Docker-Demo.git']]])
            }
        }
        
        stage('Checkout Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Checkout successfull!!!"
            }
        }
        
        stage('Run sonarqube') {
            steps {
                withSonarQubeEnv('sonarqube') {
                sh 'mvn sonar:sonar'
                }
            }
        }
        
        stage('Sonarqube Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Code Analysis successfull!!!"
            }
        }
        stage('Build') {
            steps {
                echo 'Entered inside my Build'
                sh 'mvn package -f pom.xml'
            }
            post {
				always {
					jiraSendBuildInfo branch: 'DEV-1-devops1', site: 'team-1615132910754.atlassian.net'
                }
            }
        }
        stage('Build Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Build successfull!!"
            }
        }
        
        
        stage('Upload Artifact') {
            steps {
                nexusArtifactUploader artifacts: [
		    [
		        artifactId: 'proj3', 
		        classifier: '', 
		        file: '/var/lib/jenkins/workspace/Squad1_Declarative/project/target/project-1.0-RAMA.war',
		        type: 'war'
		    ]
	    ],
    	    credentialsId: 'Nexus',
	        groupId: 'ci.jenkins.aws',
	        nexusUrl: '172.31.28.155:8081',
	        nexusVersion: 'nexus3',
	        protocol: 'http',
	        repository: 'devopsbc3',
	        version: '1.0-RAMA'
            }
        }
        stage('Nexus Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Uplocad to Artifact successfull!!"
            }
        }
    
        stage('Deploy to Test') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://172.31.92.162:8080/')], contextPath: '/mywebapp', war: '**/project-1.0-RAMA.war'
            }
        }
        
        stage('Deploy Test Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Deploy to test successfull!!"
            }
        }
        
       
        stage('Performance Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Performance test successfull!!"
            }
        }
               
		stage('Deploy to Prod') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://172.31.87.166:8080/')], contextPath: '/mywebapp', war: '**/project-1.0-RAMA.war'
            }
        }
        
        stage('Prod Deploy Notification') {
            steps {
                slackSend channel: "devopsbc3assignment", message: "SQUAD1 - Deploy to Prod successfull!!"
            }
        }
        
        
    }
}
