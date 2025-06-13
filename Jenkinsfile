pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script{
                    dockerapp = docker.build("weslley7/projeto-devops:${env.BUILD_ID}", '-f ./backend/Dockerfile ./backend  ')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        dockerapp.push('latest')
                        dockerapp.push("${env.BUILD_ID}")
                        }
                }
            }
        }

        stage('Security Scan com Trivy') {
            steps {
                sh 'trivy image weslley7/projeto-devops:latest --exit-code 0 --severity HIGH,CRITICAL --scanners vuln'
            }
        }

        stage('Deploy no Kubernetes') {
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'sed -i "s/{{tag}}/$tag_version/g" ./k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }

    }

    post {
        always {
            chuckNorris()
        }

        success {
            echo '🚀 Deploy realizado com sucesso!'
            echo '💪 Chuck Norris aprova seu pipeline DevSecOps!'
            sh 'curl -X POST -H "Content-type: application/json" --data \'{"text":"💪 Chuck Norris aprova seu pipeline DevSecOps!"}\' https://hooks.slack.com/services/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'
            echo "✅ Imagem weslley7/projeto-devops:${env.BUILD_ID} deployada no Kubernetes"
        }

        failure {
            echo '❌ Build falhou, mas Chuck Norris nunca desiste!'
            echo '🔍 Chuck Norris está investigando o problema...'
            sh 'curl -X POST -H "Content-type: application/json" --data \'{"text":"❌ Build falhou, mas Chuck Norris nunca desiste!"}\' https://hooks.slack.com/services/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'
            echo '💡 Verifique: Docker build, DockerHub push ou Kubernetes deploy'
        }

        unstable {
            echo '⚠️ Build instável - Chuck Norris está monitorando'
            sh 'curl -X POST -H "Content-type: application/json" --data \'{"text":"⚠️ Build instável - Chuck Norris está monitorando"}\' https://hooks.slack.com/services/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'
        }
    }
}