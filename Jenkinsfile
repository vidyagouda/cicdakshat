pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/vidyagouda/cicdakshat/', branch: "master"
               sh 'mvn package'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t vidyagouda/endtoendproject25may:v1 .'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'vidyagouda', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push vidyagouda/endtoendproject25may:v1'
                }
            }
        }
        
        
        stage('Deploy to k8s'){
            
            steps{
                script{
                     kubernetesDeploy (configs: 'deploymentservice.yaml' ,kubeconfigId: 'k8sconfigpwd')
                   
                }
            }
        }
    }
}
