pipeline {
    environment {
        ACR_LOGINSERVER = 'mstrdevopsacr2.azurecr.io'
        ACR_REPO    = 'mstrdevopsworkshop'
        ACR_CRED = credentials('acr-credentials')
        GIT_REPO = "https://github.com/comparexoss/cpx-oss-workshop.git"
        WEB_IMAGE="${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-web"
        API_IMAGE="${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-api"
        DB_IMAGE="${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-db"
    }
  agent any
  stages {
      stage('Checkout') {
   steps {
    git url: "${env.GIT_REPO}", branch: 'master'
   }
  }
    stage('Building api image') {
            steps{
                dir('app/api')
                {
                    script{
                    docker.build("${env.API_IMAGE}:${env.BUILD_NUMBER}")
                    }         
                }
            }
       }
    stage('Building web image') {
            steps{
                dir('app/web')
                {
                    script{
                    docker.build("${env.WEB_IMAGE}:${env.BUILD_NUMBER}", "--build-arg BUILD_DAT=`date -u +"%Y-%m-%dT%H:%M:%SZ"` --build-arg VCS_REF=`git rev-parse --short HEAD` --build-arg IMAGE_TAG_REF=v1 .")
                    }         
                }
            }
       }
    stage('Building db image') {
            steps{
                dir('app/db')
                {
                    script{
                    docker.build("${env.DB_IMAGE}:${env.BUILD_NUMBER}")
                    }         
                }
            }
       }       
    stage('Push API image to ACR') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            sh "docker push ${env.API_IMAGE}:${env.BUILD_NUMBER}"
            //sh "docker tag ${env.API_IMAGE}:${env.BUILD_NUMBER} ${env.API_IMAGE}:latest"
            //sh "docker push ${env.API_IMAGE}:latest"
            }
        }
    }
    stage('Push WEB image to ACR') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            sh "docker push ${env.WEB_IMAGE}:${env.BUILD_NUMBER}"
            //sh "docker tag ${env.WEB_IMAGE}:${env.BUILD_NUMBER} ${env.WEB_IMAGE}:latest"
            //sh "docker push ${env.WEB_IMAGE}:latest"
            }
        }
    }
    stage('Push DB image to ACR') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            sh "docker push ${env.DB_IMAGE}:${env.BUILD_NUMBER}"
            //sh "docker tag ${env.DB_IMAGE}:${env.BUILD_NUMBER} ${env.DB_IMAGE}:latest"
            //sh "docker push ${env.DB_IMAGE}:latest"
            }
        }
    }
      stage('Helm PreSteps'){
          steps{
            sh 'sudo /usr/local/bin/helm init --client-only' 
          }
      }
    stage('Helm Install WebApi') {
        steps{
               dir('app')
               {
                   sh "sudo /usr/local/bin/helm upgrade --dry-run --debug --install webapihelmd ./webapichart/ --wait --kubeconfig /home/azureuser/.kube/config --set webserver.image.repo=${env.WEB_IMAGE} --set webserver.image.tag=${env.BUILD_NUMBER} --set apiserver.image.repo=${env.API_IMAGE} --set apiserver.image.tag=${env.BUILD_NUMBER}"
                   sh "sudo /usr/local/bin/helm upgrade --install webapihelmd ./webapichart/ --wait --kubeconfig /home/azureuser/.kube/config --set webserver.image.repo=${env.WEB_IMAGE} --set webserver.image.tag=${env.BUILD_NUMBER} --set apiserver.image.repo=${env.API_IMAGE} --set apiserver.image.tag=${env.BUILD_NUMBER}"
               }
        }
    }    
     stage('Helm Install DB') {
        steps{
            dir('app')
            {
                sh "sudo /usr/local/bin/helm upgrade --dry-run --debug --install dbhelmdb ./dbchart/ --kubeconfig /home/azureuser/.kube/config --set dbserver.image.repo=${env.DB_IMAGE} --set dbserver.image.tag=${env.BUILD_NUMBER}"
                sh "sudo /usr/local/bin/helm upgrade --install dbhelmdb ./dbchart/ --kubeconfig /home/azureuser/.kube/config --set dbserver.image.repo=${env.DB_IMAGE} --set dbserver.image.tag=${env.BUILD_NUMBER}"
            }
        }
    }  
  }
}
