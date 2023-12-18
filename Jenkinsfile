pipeline {
    agent any

    parameters {
        choice(name: 'ACTION', choices: ['deploy', 'destroy'], description: 'Deploy or Destroy the Helm chart')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy Helm Chart') {
            when {
                expression { params.ACTION == 'deploy' }
            }
            steps {
                withKubeConfig(credentialsId: "taskkube") {
                    script {
                        sh 'helm upgrade --install simple-web . --namespace tornike'
                    }
                }
            }
        }

        stage('Destroy Helm Chart') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                withKubeConfig(credentialsId: "taskkube") {
                    script {
                        sh 'helm delete simple-web --namespace tornike'
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution complete.'
        }
    }
}