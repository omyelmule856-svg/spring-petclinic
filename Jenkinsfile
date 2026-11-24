
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

        stage('Test SSH Connection') {
            steps {
                withCredentials([
                    sshUserPrivateKey(
                        credentialsId: 'petclinic-deploy-key',
                        keyFileVariable: 'SSH_KEY'
                    )
                ]) {
                    sh '''
                        ssh -i "$SSH_KEY" \
                            -o StrictHostKeyChecking=accept-new \
                            -o BatchMode=yes \
                            ubuntu@172.31.16.48 \
                            'hostname && whoami'
                    '''
                }
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
