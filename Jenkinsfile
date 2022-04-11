node{
    stage('Git Checkout'){
        git branch: 'main', credentialsId: 'git-private-key', poll: false, url: 'git@github.com:andreimoldovan23/BeerConfigServer.git'
    }

    stage('Build Project'){
        sh 'mvn clean package'
    }

    stage('Build Docker Image'){
        sh 'sudo docker build -t moldoandrei/beer-config-server .'
    }

    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwd')]) {
            sh "sudo docker login -u moldoandrei -p ${dockerHubPwd}"
        }
        sh 'sudo docker push moldoandrei/beer-config-server'
    }

    stage('Run Container'){
        sh 'sudo docker rm --force configserver'
        sh 'sudo docker run -d --name configserver -e "SPRING_PROFILES_ACTIVE=localdiscovery" -p 8888:8888 moldoandrei/beer-config-server'
    }
}