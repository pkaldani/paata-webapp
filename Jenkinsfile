pipeline {
    agent any
    parameters {
        choice(name: 'WEB_ENV', choices: ['deploy', 'destroy'], description: 'Action to take for web enviroment')
    }
    environment {
        NAMESPACE = 'paata'
        PROJECT = 'paata-webapp'
    }
    stages {
        stage('init') {
            steps {
                echo "...init job..."
                sh """
                set -x -e
                if [! -f /var/lib/jenkins/.kube/config]
                then
                    az login -i
                    az aks get-credentials -n devops-interview-aks -g  devops-interview-rg
                    export KUBECONFIG=/var/lib/jenkins/.kube/config
                    kubelogin convert-kubeconfig -l msi
                else
                    export KUBECONFIG=/var/lib/jenkins/.kube/config
                fi
                echo "***variables***"
                printenv
                """
            }
        }
        stage('Deploy') {
            steps {
                echo "...Deploy job..."
                sh """
                set -x -e
                export CHART_EXSITS=\$(helm list -n ${NAMESPACE} | grep ${PROJECT} | awk '{print \$1}')
                export CHART_VER=\$(cat ./helm/paata-webapp/Chart.yaml | grep version | awk '{print \$2}')
                
                helm package ./helm/${PROJECT}
                if [ -n \${CHART_EXSITS} ]
                then
                    helm upgrade ${PROJECT} ${PROJECT}-\${CHART_VER}.tgz -n ${NAMESPACE}
                else
                    helm install ${PROJECT} ${PROJECT}-\${CHART_VER}.tgz -n ${NAMESPACE}
                fi

                """
            }
        }
    }
}