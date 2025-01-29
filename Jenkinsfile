pipeline {
    agent any
    
    environment{
        dockerhubpwd = credentials('dockerhubpwd')
    }

    tools {
        jdk 'JDK-11'       // Use JDK 11
        maven 'Maven-3'    // Use Maven
    }

    stages {
        stage('Clone or Checkout Code') {
            steps {
              #  git url:"https://github.com/HandsOnDevOpsTraining/java-unit-tests-maven.git", branch:"main"
            }
        }

       stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
}
