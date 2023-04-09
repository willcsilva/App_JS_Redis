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
                sh 'docker build -t devops/app .'
            
            }
        }
        stage('Deploy no Cluster Swarm'){
            steps{
                sshagent(['Docker_Swarm_Manager-SSH-Agent']) {
                 sh 'scp -o StrictHostKeyChecking=no docker-compose.yml vagrant@192.168.100.26:'
                 sh 'sh -o StrictHostKeyChecking=no vagrant@192.168.100.26 docker stack deploy --prune --compose-file docker-compose.yml devops/app'
                }
            }
        }
        stage('Sleep para subir os containers'){
            steps{
                sh 'sleep 10'
            }
        }
    }
}