pipeline {
    agent any
    parameters {
        choice(name: 'WEB_ENV', choices: ['deploy', 'destroy'], description: 'Action to take for web enviroment')
    }
    stages {
        stage('init') {
            steps {
                echo "...init job..."
                sh """
                set -x -e
                az login -i
                az aks get-credentials -n devops-interview-aks -g  devops-interview-rg
                export KUBECONFIG=/var/lib/jenkins/.kube/config
                kubelogin convert-kubeconfig -l msi
                echo "init kubectl"
                kubectl get pods -n paata
                """
            }
        }
        stage('Deploy') {
            steps {
                echo "...Deploy job..."
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