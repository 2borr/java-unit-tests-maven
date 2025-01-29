pipeline {
    agent any
    
    environment{
        dockerhubpwd = credentials('dockerhubpwd')
    }

    tools {
        jdk 'JDK-11'       // Use JDK 11
        maven 'maven'    // Use Maven
    }

    stages {
       

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
}
