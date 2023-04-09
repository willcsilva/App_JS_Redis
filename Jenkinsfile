pipeline {
    agent any
    stages{
        stage('Docker-Swarm Connection'){
            steps{
                sshagent(['Docker_Swarm_Manager-SSH-Agent']) {
                 sh "ssh -o StrictHostKeyChecking=no vagrant@192.168.100.26" 
            }
            }
        }
        //stage('Docker Remove APP AlreadyExists'){
        //    steps{
        //        sshagent(['Docker_Swarm_Manager-SSH-Agent']) {
        //         sh "ssh -o StrictHostKeyChecking=no vagrant@192.168.100.26 docker service rm helloworld"
        //         sh "ssh -o StrictHostKeyChecking=no vagrant@192.168.100.26 docker service rm redis"
        //    }
        //    }
        //}
        stage('Build da Imagem docker'){
            steps{
                sh 'docker build -t willcsilva/node-js:latest .'
            
            }
        }
        stage('Deploy no Cluster Swarm'){
            steps{
                sshagent(['Docker_Swarm_Manager-SSH-Agent']) {
                 sh 'scp -o StrictHostKeyChecking=no docker-compose.yml vagrant@192.168.100.26:'
                 sh 'ssh -o StrictHostKeyChecking=no vagrant@192.168.100.26 docker stack deploy --compose-file docker-compose.yml node-js:latest'
                }
            }
        }
        stage('DockerHub_Push'){
            steps{
            withCredentials([string(credentialsId: 'Docker_Hub_Pswd', variable: 'Docker_Hub_Pswd')]) {
              sh 'docker login -u willcsilva -p ${Docker_Hub_Pswd}'
            }
            sh 'docker push willcsilva/node-js:latest'
            }
        }
        //stage('Removendo a imagem localmente'){
        //    steps{
        //      sh 'docker stack rm willcsilva/node-js:latest'
        //    }
        //}
        stage('Sleep para subir os containers'){
            steps{
                sh 'sleep 10'
            }
        }
    }
}