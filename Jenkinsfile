pipeline {
    agent { label 'Dev-Agent node' }
    
    stages{
        stage('Checkout'){
            steps{
                git url: 'https://github.com/akashbkochure/Project-on-Building-and-Deploying-a-Node.js-Application-with-Docker-on-Ubuntu.git', branch: 'master'
            }
        }
        stage('Build'){
            steps{
                sh 'sudo docker rmi -f akashbkochure/nodo-todo-app-test:latest'
                sh 'sudo docker build . -t akashbkochure/nodo-todo-app-test:latest'
            }
        }
        stage('Test image') {
            steps {
                echo 'testing...'
                sh 'sudo docker inspect --type=image akashbkochure/nodo-todo-app-test:latest '
            }
        }
        
        stage('Push'){
            steps{
        	     sh "sudo docker login -u akashbkochure -p *******"
                 sh 'sudo docker push akashbkochure/nodo-todo-app-test:latest'
            }
        }  
        stage('Deploy'){
            steps{
                echo 'deploying on another server'
                sh 'sudo docker stop nodetodoapp || true'
                sh 'sudo docker rm nodetodoapp || true'
                sh 'sudo docker run -d --name nodetodoapp -p 8000:8000 akashbkochure/nodo-todo-app-test:latest'
                sh '''
                ssh -i akash.pem -o StrictHostKeyChecking=no ubuntu@184.73.150.150<<EOF
                sudo docker login -u akashbkochure -p *********
                sudo docker rmi -f akashbkochure/nodo-todo-app-test:latest
                sudo docker pull akashbkochure/nodo-todo-app-test:latest
                sudo docker stop nodetodoapp || true
                sudo docker rm nodetodoapp || true 
                sudo docker run -d --name nodetodoapp -p 8000:8000 akashbkochure/nodo-todo-app-test:latest
                '''
            }
        }
    }
}
