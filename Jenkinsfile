pipeline {
    agent any
    tools{
        maven 'maven3'
        jdk 'jdk17'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('Git') {
            steps {
                git 'https://github.com/subho153/secretsanta-generator.git'
            }
        }
        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Code Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Trivy Scan') {
            steps {
                sh 'trivy fs --format table -o fs.html .'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Petclinic \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=Petclinic '''
    
                }
            }
        }
        stage('Docker Build & push') {
            steps {
                script{
                  withDockerRegistry(credentialsId: 'dockerconn') {
                        sh 'docker build -t springbootwebapp .'
                        sh 'docker tag springbootwebapp dockerhubdemosubha1234/springwebappnew'
                        sh 'docker push dockerhubdemosubha1234/springwebappnew'
                        sh 'docker run -d --name springbootwebapp -p 8089:8089 dockerhubdemosubha1234/springwebappnew'
                    }  
                }
            }
        }
    }
}
