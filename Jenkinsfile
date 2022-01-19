pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps { 
                sh "echo 'pulling'"
                sh "git pull"
            }
        }
        stage('Test'){
            steps {
                sh "echo 'Testing...'" 
            }
        }
        stage('Deploy') {
            steps {
                sh "helm install ./indiana-game-app"
            }
        }
    }
}
