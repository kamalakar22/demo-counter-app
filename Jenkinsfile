pipeline {
    agent any 
    
    tools {
        maven 'maven'
    }
    
    stages {
        
        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main', credentialsId: '10376970-92a5-4331-a1cb-20432b66cb7d',
                        url: 'https://github.com/kamalakar22/demo-counter-app.git'
                }
            }
        }
        
        stage('UNIT testing') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        
        stage('Integration testing') {
            steps {
                script {
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
       
        stage('Maven build') {
            steps {
                script {
                    sh "mvn clean install"
                }
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        
        stage('Sonar Scan') {
            steps {
                withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                    sh 'mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}'
                }
            }
        }
        
        stage('Nexus Upload') {
            steps {
              script{
                  def readPomVersion = readMavenPom file: 'pom.xml'
                  nexusArtifactUploader artifacts: [[artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']], 
                  credentialsId: 'nexus', 
                  groupId: 'com.example',
                  nexusUrl: '34.229.49.204:8081', 
                  nexusVersion: 'nexus3',
                  protocol: 'http', 
                  repository: 'maven-central-repo',
                  version: "${readPomVersion.version}"
              }
            }
        }
    }
}
