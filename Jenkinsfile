pipeline {
      agent any
      environment {
            Service = "quiz-app-frontend"
            Repo_TAG = "${dockerhubname}/${Service}:${BUILD_ID}"
      }
      stages {
            stage('Cloning the repository') {
                  steps {
                        git credentialsId: 'Github', url: "https://github.com/${githubname}/${Service}"
                  }
            }
            stage('Build') {
                  steps {
                        sh 'docker image build -t ${Repo_TAG} .'
                  }
            } 
             stage('Push to the docker hub registry') {
                  steps {
                        withCredentials([string(credentialsId: 'dockerhubbb', variable: 'dockerhubbbb')]) {
                              sh "docker login -u ervisa -p ${dockerhubbbb}"
                        }
                        sh 'docker push ${Repo_TAG}'
                  }
             }
            stage('Deploy service to Cluster') {
                  steps {
                        sh 'envsubst < ${WORKSPACE}/frontend.yaml | kubectl apply -f -'
                  }
            }
            stage('Deploy ingress to Cluster') {
                  steps {
                        sh 'envsubst < ${WORKSPACE}/ingress.yaml | kubectl apply -f -'
                  }
            }            
      }
}
