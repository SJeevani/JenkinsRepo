pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/SJeevani/JenkinsRepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat "docker build -t sms-img -f Dockerfile ."
            }
        }

//         stage('Run Container'){
//         steps {
//                 bat "docker run -d -p 8100:8100 --name sms_container sms-img:latest"
//             }
//         }

        stage('K8s Deployment') {
            steps {
                script {
                    withEnv(["KUBECONFIG=C:/Users/sabbi/.kube/config"]) {

                        bat "kubectl apply -f namespace.yaml --validate=false"
                        bat "kubectl apply -f deployment.yaml --validate=false"
                        bat "kubectl apply -f service.yaml --validate=false"

                    }
                }
            }
        }
    }

    post {
        success {
            echo "✔ Completed: Build and Deploy on Windows"
        }
        failure {
            echo "❌ Pipeline Failed"
        }
    }
}