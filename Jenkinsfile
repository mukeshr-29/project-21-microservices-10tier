pipeline {
    agent any
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mukeshr-29/project-21-microservices-10tier.git'
            }
        }
        stage('sonarqube analysis') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token', installationName: 'sonar-server') {
                        sh '''
                            $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=microservices -Dsonar.projectName=microservices -Dsonar.java.binaries=. 
                        '''
                    }
                }
            }
        }
        stage('quality gate'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage('adservices'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/adservice/'){
                            sh 'docker build -t mukeshr29/adservice:latest .'
                            sh 'docker push mukeshr29/adservice:latest'
                            sh 'docker rmi mukeshr29/adservice:latest'
                        }
                    }
                }
            }
        }
        stage('cartservices'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/cartservice/src/'){
                            sh 'docker build -t mukeshr29/cartservice:latest .'
                            sh 'docker push mukeshr29/cartservice:latest'
                            sh 'docker rmi mukeshr29/cartservice:latest'
                        }
                    }
                }
            } 
        }
        stage('checkoutservices'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/checkoutservice/'){
                            sh 'docker build -t mukeshr29/checkoutservice:latest .'
                            sh 'docker push mukeshr29/checkoutservice:latest'
                            sh 'docker rmi mukeshr29/checkoutservice:latest'
                        }
                    }
                }
            }
        }
        stage('currencyservices'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/currencyservice/'){
                            sh 'docker build -t mukeshr29/currencyservice:latest .'
                            sh 'docker push mukeshr29/currencyservice:latest'
                            sh 'docker rmi mukeshr29/currencyservice:latest'
                        }
                    }
                }
            }
        }
        stage('emailservices'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/emailservice/'){
                            sh 'docker build -t mukeshr29/emailservice:latest .'
                            sh 'docker push mukeshr29/emailservice:latest'
                            sh 'docker rmi mukeshr29/emailservice:latest'
                        }
                    }
                }
            }
        }
        stage('frontend'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/frontend/'){
                            sh 'docker build -t mukeshr29/frontend:latest .'
                            sh 'docker push mukeshr29/frontend:latest'
                            sh 'docker rmi mukeshr29/frontend:latest'
                        }
                    }
                    
                }
            }
        }
        stage('loadgenerator'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/loadgenerator/'){
                            sh 'docker build -t mukeshr29/loadgenerator:latest .'
                            sh 'docker push mukeshr29/loadgenerator:latest'
                            sh 'docker rmi mukeshr29/loadgenerator:latest'
                        }
                    }
                }
            }
        }
        stage('paymentservice'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/paymentservice/'){
                            sh 'docker build -t mukeshr29/paymentservice:latest .'
                            sh 'docker push mukeshr29/paymentservice:latest'
                            sh 'docker rmi mukeshr29/paymentservice:latest'
                        }
                    }
                }
            }
        }
        stage('productcatalogservice'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/productcatalogservice'){
                            sh 'docker build -t mukeshr29/productcatalogservice:latest .'
                            sh 'docker push mukeshr29/productcatalogservice:latest'
                            sh 'docker rmi mukeshr29/productcatalogservice:latest'
                        }
                    }
                }
            }
        }
        stage('recommendationservice'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/recommendationservice/'){
                            sh 'docker build -t mukeshr29/recommendationservice:latest .'
                            sh 'docker push mukeshr29/recommendationservice:latest'
                            sh 'docker rmi mukeshr29/recommendationservice:latest'
                        }
                    }
                }
            }
        }
        stage('shippingservice'){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'dockerhub', toolName: 'docker'){
                        dir('/var/lib/jenkins/workspace/project-21/src/shippingservice/'){
                            sh 'docker build -t mukeshr29/shippingservice:latest .'
                            sh 'docker push mukeshr29/shippingservice:latest'
                            sh 'docker rmi mukeshr29/shippingservice:latest'
                        }
                    }
                }
            }
        }
        stage('k8s deploy'){
            steps{
                withKubeConfig(caCertificate: '', clusterName: 'my-eks', contextName: '', credentialsId: 'k8s', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://4A7BE8280B1DF32A482AFBC0165A0709.gr7.us-east-1.eks.amazonaws.com'){
                    sh 'kubectl apply -f deployment-service.yml'
                    sh 'kubectl get pods'
                    sh 'kubectl get svc'
                }
            }
        }      
    }
}
