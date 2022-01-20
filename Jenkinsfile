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
                    AWS("eks update-kubeconfig --name even-more-ridiculous-unicorn --region eu-central-1")
                    }
                sh "echo 'installing eksctl'"
                sh "curl --silent --location 'https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz' | tar xz -C /tmp"
                sh "mv /tmp/eksctl /usr/local/bin"
                sh "apt-get install -y apt-transport-https ca-certificates curl"
                sh "curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg"
                sh "echo 'deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main' | tee /etc/apt/sources.list.d/kubernetes.list"
                sh "apt-get update"
                sh "apt-get install -y kubectl"
                sh "echo 'installing helm'"
                sh "curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3"
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
                sh "helm install ./indiana-game-app --namespace indiana --generate-name"
            }
        }
    }
}
