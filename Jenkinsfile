pipeline {
    agent any
    

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
