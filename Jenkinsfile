pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn -B test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn -B package -DskipTests'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar',
                    fingerprint: true
            }
        }

        stage('Deploy') {
    steps {
        withCredentials([
            sshUserPrivateKey(
                credentialsId: 'petclinic-deploy-key',
                keyFileVariable: 'SSH_KEY'
            )
        ]) {
            sh '''
                echo "Deploying PetClinic to Application EC2"

                scp -i "$SSH_KEY" \
                    -o StrictHostKeyChecking=accept-new \
                    target/spring-petclinic-4.0.0-SNAPSHOT.jar \
                    ubuntu@172.31.40.147:/opt/petclinic/

                ssh -i "$SSH_KEY" \
                    -o StrictHostKeyChecking=accept-new \
                    ubuntu@172.31.40.147 \
                    'sudo systemctl restart petclinic'

                echo "Deployment completed"
            '''
        }
    }
}
    }
}
