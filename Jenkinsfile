pipeline {
    agent any
    parameters {
        choice(name: 'helm_chart', choices: ['deploy', 'destroy'], description: 'Action to take for web')
    }
    environment {
        NAMESPACE = 'paata'
        PROJECT = 'paata-webapp'
    }
    stages {
        stage('init') {
            steps {
                echo "****init job****"
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
                script {
                    env.CHART_EXSITS = sh (
                        script: "helm list -n ${NAMESPACE} | grep ${PROJECT} | awk '{print \$1}'",
                        returnStdout: true
                        ).trim()
                }
                echo "${env.CHART_EXSITS}"
            }
        }
        stage('Deploy') {
        when {
            expression { params.helm_chart == 'deploy'}
            }
            steps {
                echo "****Deploy job****"
                sh """
                set -x -e
                export CHART_VER=\$(cat ./helm/paata-webapp/Chart.yaml | grep version | awk '{print \$2}')
                
                helm package ./helm/${PROJECT}

                if [ ${env.CHART_EXSITS} =='' ]; then
                    helm install ${PROJECT} ${PROJECT}-\${CHART_VER}.tgz -n ${NAMESPACE}
                else
                    helm upgrade ${PROJECT} ${PROJECT}-\${CHART_VER}.tgz -n ${NAMESPACE}  
                fi

                """
            }
        }
        stage('Destroy') {
        when {
            expression { params.helm_chart == 'destroy'}
            }
            steps {
                echo "****Destroy job****"
                sh """
                set -x -e
                if [ ${env.CHART_EXSITS} == ${PROJECT} ]; then
                    helm uninstall ${PROJECT} -n ${NAMESPACE}
                else
                    echo "Helm Chart does not exists"
                fi

                """
            }
        }
    }
}