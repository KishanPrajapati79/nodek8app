pipeline {
    agent any
    
    tools {
        nodejs "Nodejs"
    }

    stages {
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/KishanPrajapati79/nodek8appecr.git'
            }
        }
        stage('Installing dependencies') {
            steps {
                dir('DockerWebappK8/app') {
                    sh 'npm install'
                    // sh 'npm test'
                }
            }
        }
        
        // stage('Building and pushing Docker image to AWS EKS') {
        //     steps {
        //         // Build Docker image using Dockerfile
        //         dir('DockerWebappK8') {
        //             script {
        //                 withCredentials([[
        //                     $class: 'AmazonWebServicesCredentialsBinding',
        //                     credentialsId: 'awsecraccess',
        //                     accessKeyVariable: 'AWS_ACCESS_KEY_ID',
        //                     secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
        //                 ]]) {
        //                         withDockerRegistry(credentialsId: '778b21cb-c4a2-4f65-9bc8-0792220228d1') {
        //                             sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/w3i0n9m9"
        //                             sh "docker build -t public.ecr.aws/w3i0n9m9/nodek8appecr:latest ."
        //                             sh "docker push public.ecr.aws/w3i0n9m9/nodek8appecr:latest"
        //                         }
        //                     }
        //             }
        //         }
        //     }
        // }
        // stage('Deploy to Kubernetes') {
        //     steps {
        //         dir('K8demo') {
        //             withCredentials([
        //                 file(credentialsId: 'k8cacert', variable: 'CA_CERT_PATH'),
        //                 file(credentialsId: 'k8clientkey', variable: 'CLIENT_KEY_PATH'),
        //                 file(credentialsId: 'k8clientcert', variable: 'CLIENT_CERT_PATH'),
        //                 kubeconfigContent(credentialsId: 'k8configcopycopy', variable: 'KUBECONFIG_CONTENT')
        //             ]) {
        //                 script {
        //                     def kubeconfig = env.KUBECONFIG_CONTENT
        //                     kubeconfig = kubeconfig.replaceAll('/home/kishan/.minikube/ca.crt', env.CA_CERT_PATH)
        //                     kubeconfig = kubeconfig.replaceAll('/home/kishan/.minikube/profiles/minikube/client.crt', env.CLIENT_CERT_PATH)
        //                     kubeconfig = kubeconfig.replaceAll('/home/kishan/.minikube/profiles/minikube/client.key', env.CLIENT_KEY_PATH)
        //                     writeFile file: 'kubeconfig', text: kubeconfig
        //                     env.KUBECONFIG = 'kubeconfig'
        //                     sh 'kubectl get pods'
        //                     sh 'kubectl apply -f mongo-config.yaml'
        //                     sh 'kubectl apply -f mongo-secret.yaml'
        //                     sh 'kubectl apply -f mongo.yaml'
        //                     sh 'kubectl apply -f webapp.yaml'
        //                 }
        //             }
        //         }
        //     }
        // }
        
    }
    post {
            success {
                echo 'Pipeline was successfully executed and application deployed'
            }
            failure {
                echo 'Something went wrong, pipeline failed'
            }
            unstable {
                echo 'Pipeline is unstable please check'
            }
            changed {
                echo 'Pipeline changed from previous configuration'
            }
            always {
                emailext (
                    subject: "Pipeline status: ${BUILD_NUMBER}",
                    body: '''<html>
                                <body>
                                    <p>Pipeline: ${PROJECT_NAME}</p>
                                    <p>Build Status: ${BUILD_STATUS}</p>
                                    <p>Build Status: ${BUILD_NUMBER}</p>
                                    <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                                </body>
                            </html>''',
                    to: 'prajapatikishan.kp2002@gmail.com',
                    from: 'jenkins@example.com',
                    replyTo: 'jenkins@example.com',
                    mimeType: 'text/html'
                )
            }
    }
}
