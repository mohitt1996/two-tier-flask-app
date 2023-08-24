pipeline{
    agent{label 'Flask_todo'}
    
    stages{
        stage('code'){
            steps{
                git url:'https://github.com/mohitt1996/two-tier-flask-app.git', branch:'master'
            }
        }
        stage('build & test'){
            steps{
                sh 'docker build . -t node-todo'
            }
        }
        stage('image push to dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "Github", passwordVariable: "dockerHubPass", usernameVariable: "dockerhubUsr")]) {
                    sh "docker login -u ${dockerhubUsr} -p ${dockerHubPass}"
                    sh "docker tag node-todo ${dockerhubUsr}/node-todo:latest"
                    sh "docker push ${dockerhubUsr}/node-todo:latest"
                }
            }
        }
        stage('deploy'){
            steps{
                sh 'docker-compose down'
                sh "docker-compose up -d --no-deps --build backend"
                
                
            }
        }
    }
}
