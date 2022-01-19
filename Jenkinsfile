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
                sh "echo 'installing the aws cli'"
                sh "curl 'https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip' -o 'awscliv2.zip'"
                sh "unzip awscliv2.zip"
                sh "./aws/install --update"
                sh "echo 'configuring aws cli'"
                sh "aws configure set aws_access_key_id AKIA4WJYQSNFGSU76VNA"
                sh "aws configure set aws_secret_access_key 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"
                sh "aws configure set region eu-central-1"
                sh "aws configure set output json"
                sh "echo 'installing eksctl'"
                sh "curl --silent --location 'https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_Linux_amd64.tar.gz' | tar xz -C /tmp"
                sh "mv /tmp/eksctl /usr/local/bin"
                sh "echo 'connecting the eksctl to cluster'"
                sh "aws eks update-kubeconfig --name basic-cluster --region eu-central-1"
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
                sh "helm install ./indiana-game-app"
            }
        }
    }
}
