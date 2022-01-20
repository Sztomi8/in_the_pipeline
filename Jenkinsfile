@Library('github.com/releaseworks/jenkinslib') _

pipeline { 
    agent any 
    stages {
        stage('Build') { 
            steps { 
                sh "echo updating apt"
                sh "apt-get update"
                sh "echo installing zip"
                sh "apt install zip"
                sh "echo 'pulling'"
                sh "git pull"
                sh "echo 'configuring aws cli'"
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'aws_key', 
                usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    AWS("eks update-kubeconfig --name ridiculous-unicorn-1642607251 --region eu-central-1")
                    }
                sh "echo 'installing eksctl'"
                sh "curl --silent --location 'https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz' | tar xz -C /tmp"
                sh "mv /tmp/eksctl /usr/local/bin"
                sh "echo 'installing helm'"
                sh "curl -LO https://git.io/get_helm.sh"
                sh "chmod 700 get_helm.sh"
                sh "./get_helm.sh"
            }
        }
        stage('Test'){
            steps {
                sh "echo 'Testing...'" 
            }
        }
        stage('Deploy') {
            steps {
                sh "echo 'deploying with helm'"
                sh "helm init --debug"
                sh "helm install indiana ./indiana-game-app/Chart.yaml"
            }
        }
    }
}
