pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sureshrajuvetukuri/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t venkateshsadamastula/carloan .'
                }
            }
        }
        stage('Push Image to Docker Hub') {
          steps {
           sh    withCredentials([string(credentialsId: 'DOCKER', variable: 'passwd')]) {
           sh    'docker login -u venkateshsadamastula -p ${passwd}'
           sh    'docker push venkateshsadamastula/carloan'
           }
          }
        }
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    
    }    
}
