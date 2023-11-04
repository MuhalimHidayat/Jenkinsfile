pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 4000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                input message: 'Lanjutkan ke tahap Deploy?', parameters: [choice(choices: 'Proceed\nAbort', description: 'Pilih untuk melanjutkan atau menghentikan pipeline', name: 'APPROVAL')]
            }
        }
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the React App? (Click "Proceed" to continue)'
                script {
                    sleep(60)
                }
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}