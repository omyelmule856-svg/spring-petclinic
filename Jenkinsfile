
pipeline {
    agent any

    stages {

        stage('Build & Test') {
            steps {
                echo 'Building and testing PetClinic application'
                sh 'mvn -B clean verify'
            }
        }

        stage('Archive') {
            steps {
                echo 'Archiving PetClinic JAR'
                archiveArtifacts artifacts: 'target/*.jar',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'PetClinic CI Pipeline completed successfully!'
        }
        failure {
            echo 'PetClinic CI Pipeline failed!'
        }
    }
}
