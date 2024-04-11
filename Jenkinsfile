pipeline {
    agent any
    parameters {
        choice(name: 'WEB_ENV', choices: ['deploy', 'destroy'], description: 'Action to take for web enviroment')
    }
    stages {
        stage('Deploy') {
            steps {
                echo 'Hello,'
            }
        }
    }
}