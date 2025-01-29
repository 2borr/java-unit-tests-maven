pipeline {
    agent any
    
     environment {
        SONARQUBE_URL = 'http://192.168.56.1:9000'
        SONAR_TOKEN = credentials('sonar-token')
        DOCKER_IMAGE = "asid55/mara:latest"
        DOCKER_CREDENTIALS_ID = "docker-hub-credentials"
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

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker rmi ${DOCKER_IMAGE}"
            }
        }
    }

    post {
        success {
            echo 'Docker Image Successfully Built and Pushed!'
        }
        failure {
            echo 'Build or Push Failed!'
        }
}
}
