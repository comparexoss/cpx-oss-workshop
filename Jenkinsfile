pipeline {
    environment {
        ACR_LOGINSERVER = 'mstrdevopsacr2.azurecr.io'
        ACR_REPO    = 'mstrdevopsworkshop'
        ACR_CRED = credentials('acr-credentials')
        GIT_REPO = "https://github.com/comparexoss/cpx-oss-workshop.git"
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
                    docker.build("${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-api:${env.BUILD_NUMBER}")
                    }         
                }
            }
       }
    stage('Building web image') {
            steps{
                dir('app/web')
                {
                    script{
                    docker.build("${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-web:${env.BUILD_NUMBER}")
                    }         
                }
            }
       }
    stage('Building db image') {
            steps{
                dir('app/db')
                {
                    script{
                    docker.build("${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-db:${env.BUILD_NUMBER}")
                    }         
                }
            }
       }       
    stage('Push API image to ACR') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            sh "docker push ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-api:${env.BUILD_NUMBER}"
            sh "docker tag ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-api:${env.BUILD_NUMBER} ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-api:latest"
            sh "docker push ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-api:latest"
            }
        }
    }
    stage('Push WEB image to ACR') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            sh "docker push ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-web:${env.BUILD_NUMBER}"
            sh "docker tag ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-web:${env.BUILD_NUMBER} ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-web:latest"
            sh "docker push ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-web:latest"
            }
        }
    }
    stage('Push DB image to ACR') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            sh "docker push ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-db:${env.BUILD_NUMBER}"
            sh "docker tag ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-db:${env.BUILD_NUMBER} ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-db:latest"
            sh "docker push ${env.ACR_LOGINSERVER}/${env.ACR_REPO}/rating-db:latest"
            }
        }
    }
    stage('Helm Deploy') {
        steps{
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'acr-credentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
            sh "docker login ${env.ACR_LOGINSERVER} -u $USERNAME -p $PASSWORD"
            }
        }
    }    
  }
}
