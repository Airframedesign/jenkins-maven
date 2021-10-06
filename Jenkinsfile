pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {
                sh "mvn clean compile"
            }
        }
        stage('Test') { 
            steps {
                sh "mvn test site"
            }  
        
        post {
            always {
                junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
        }
        }
        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }
        
        stage('Build and Docker Image') { 
            steps {
                sh "docker build -t airframedesign/docker_jenkins_pipeline:${BUILD_NUMBER} ."
            }
        }
        
        stage('Docker login') { 
            steps {
                withCredentials([string(credentialsId: 'DockerNewID', variable: 'Dockerpwd')]) {
                     sh "docker login -u airframedesign -p ${Dockerpwd}"
            }
        }
        }
        stage('Push to Docker') { 
            steps {
                sh "docker push airframedesign/docker_jenkins_pipeline:${BUILD_NUMBER}"
            }
        }
        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}
