pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Source code is available!'
            }
        }

        stage('Build') {
            steps {
                echo 'Building PetClinic application!'
            }
        }

        stage('Test') {
            steps {
                echo 'Running automated tests!'
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
                    ubuntu@172.31.40.147 \
                    'hostname && whoami'
            '''
        }
    }
}
    }
}
