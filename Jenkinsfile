pipeline {
    agent any
    stages {
        stage('Install kubectl locally') {
            steps {
                script {
                   sh '''
                # Descargar kubectl
                curl -LO "https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl"
                chmod +x kubectl

                # Mover kubectl a $HOME/bin
                mkdir -p $HOME/bin
                mv kubectl $HOME/bin/

                # Agregar $HOME/bin al PATH
                export PATH=$HOME/bin:$PATH
                '''
                }
            }
        }
        stage('Verify kubectl') {
            steps {
                 sh '''
                  export PATH=$HOME/bin:$PATH
                kubectl version --client
                '''
               

            }
        }
        stage('Execute kubectl commands') {
            steps {
              withKubeConfig([credentialsId: 'minikube-token', serverUrl: 'https://192.168.49.2:8443']) {
                    sh """
                    export PATH=$HOME/bin:$PATH
                    # Verificar la conexión con el cluster
                    kubectl cluster-info

                    # Listar los namespaces
                    kubectl get namespaces

                    # Listar los pods en el namespace 'default'
                    kubectl get pods -n default
                    kubectl get pods -n jenkins
                    ls
                
                    
                    """
                        git url: 'https://github.com/erixrivas/draft-proyectofinal.git', credentialsId: 'githuberix', branch: 'add-prom-graf'
                }
            }}
        stage('deploy dev') {
            steps {
                git url: 'https://github.com/erixrivas/draft-proyectofinal.git', credentialsId: 'githuberix', branch: 'add-prom-graf'
                withKubeConfig([credentialsId: 'minikube-token', serverUrl: 'https://192.168.49.2:8443']) {
                sh '''
                export PATH=$HOME/bin:$PATH
                kubectl version --client
                kubectl -n dev apply -f k8s/dev/api/api-deployment.yaml,k8s/dev/api/api-service.yaml
                kubectl -n dev apply -f k8s/dev/web/web-deployment.yaml,k8s/dev/web/web-service.yaml,k8s/dev/web/web-ingress.yaml
                '''
                }
            }
            
        }
        
        stage('deploy qa') {
            steps {
                git url: 'https://github.com/erixrivas/draft-proyectofinal.git', credentialsId: 'githuberix', branch: 'add-prom-graf'
                withKubeConfig([credentialsId: 'minikube-token', serverUrl: 'https://192.168.49.2:8443']) {
                sh '''
                export PATH=$HOME/bin:$PATH
                kubectl version --client
                kubectl -n qa apply -f k8s/qa/api/api-deployment.yaml,k8s/qa/api/api-service.yaml
                kubectl -n qa apply -f k8s/qa/web/web-deployment.yaml,k8s/qa/web/web-service.yaml,k8s/qa/web/web-ingress.yaml
                '''
                }
            }
            
        }

        stage('deploy prod') {
            steps {
                git url: 'https://github.com/erixrivas/draft-proyectofinal.git', credentialsId: 'githuberix', branch: 'add-prom-graf'
                withKubeConfig([credentialsId: 'minikube-token', serverUrl: 'https://192.168.49.2:8443']) {
                sh '''
                export PATH=$HOME/bin:$PATH
                kubectl version --client
                kubectl -n prod apply -f k8s/prod/api/api-deployment.yaml,k8s/prod/api/api-service.yaml
                kubectl -n prod apply -f k8s/prod/web/web-deployment.yaml,k8s/prod/web/web-service.yaml,k8s/prod/web/web-ingress.yaml
                '''
                }
            }
          
        }


        }
    }