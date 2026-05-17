 pipeline{
    agent any
    stages{
        stage("1. build images"){
            steps{
               bat "docker build -t image-frontend ./frontend:${BUILD_NUMBER}" 
                bat "docker build -t image-backend ./backend:${BUILD_NUMBER}"
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Build images executed successfully========"
                }
                failure{
                    echo "========Build images execution failed========"
                }
            }
        }
    
        stage("2. load image to minikube"){
             steps{
                bat "minikube image load image-frontend:${BUILD_NUMBER}"
                bat "minikube image load image-backend:${BUILD_NUMBER}"
            }
            
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Run containers executed successfully========"
                }
                failure{
                    echo "========Run containers execution failed========"
                }
            }
        }
    
        stage("3. update deployments and rollout"){
            steps{
                bat "kubectl set image deployment/inventory-frontend-app frontend-app=image-frontend:${BUILD_NUMBER}"
                bat "kubectl set image deployment/inventory-backend java-app=image-backend:${BUILD_NUMBER}"
                bat "kubectl rollout restart deployment/inventory-frontend-app"
                bat "kubectl rollout restart deployment/inventory-backend"
            }
        }
    }
    
}
