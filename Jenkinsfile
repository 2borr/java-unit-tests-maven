pipeline {
    agent any
    
     environment {
        SONARQUBE_URL = 'http://192.168.56.1:9000'
        SONAR_TOKEN = credentials('sonar-token')
    }


    tools {
        jdk 'JDK-11'       // Use JDK 11
        maven 'maven'    // Use Mavenn
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

         stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.host.url=$SONARQUBE_URL -Dsonar.login=$SONAR_TOKEN'
                }
            }
        }

        stage('Archive Artifacts') { // Separate stage for archiving
            steps {
                archiveArtifacts(
                    artifacts: 'target/*.jar, build/libs/*.war', // Files to archive (comma-separated)
                    allowEmptyArchive: true, // Optional: Allow archiving even if no files match
                    // Optional: If you want to keep the build even if archiving fails
                    // failOnError: false,
                    // Optional: If you want to only archive successful builds
                    // onlyIfSuccessful: true,
                    // Optional: If you want to use a different archive root directory
                    // archiveRoot: './'
                )
            }
        }
}
}
