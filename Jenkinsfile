pipeline {
    agent any
    parameters {
        choice(name: 'WEB_ENV', choices: ['deploy', 'destroy'], description: 'Action to take for web enviroment')
    }
    stages {
        stage('init') {
            steps {
                sh """
                set -x -e
                az aks get-credentials -n devops-interview-aks -g  devops-interview-rg
                export KUBECONFIG=~/.kube/config
                kubelogin convert-kubeconfig -l msi
                echo "init kubectl"
                kubectl get pods -n paata
                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                set -x -e
                echo "......deploy kubectl"
                kubectl get pods -n paata

                """
                echo 'Hello,'
            }
        }
    }
}