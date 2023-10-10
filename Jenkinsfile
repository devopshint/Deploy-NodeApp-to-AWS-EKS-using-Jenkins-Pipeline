pipeline {
  agent any
    
  tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'githubwithpassword', url: 'https://github.com/devopshint/kubernetes_jenkins_pipeline.git']]])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t devopshint/node-app-1.0 .'
                }
            }
        }


        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u devopshint -p ${dockerhubpwd}'
                 }  
                 sh 'docker push devopshint/node-app-1.0'
                }
            }
        }
         
     stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "nodeapp-deployment-service.yml", kubeconfigId: "kubernetes_config")
        }
      }
    }

  }
}
