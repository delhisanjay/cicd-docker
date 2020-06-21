node{
    stage("SCM Checkout"){
        git credentialsId: 'git-creds', url: 'https://github.com/delhisanjay/cicd-docker'
    }
    stage('Mvn Package'){
        def mvnHome = tool name: 'maven-3', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh "${mvnCMD} clean package"
    }
    stage('Build Docker Image'){
        sh 'docker build -t delhisanjay/my-app:2.0.0'
    }
    stage('Push Docker Image'){
        withCredentials([string(credentialID: 'docker-pwd', variable: 'dockerHubPwd')]){
            sh "docker login -u delhisanjay -p ${dockerHubPwd}"
        }
        sh 'docker PUSH delhisanjay/my-app:2.0.0'
    }
    stage('Run Container on Dev Server'){
        def dockerRun = 'docker run -p 8080:8080 -d --name my-app delhisanjay/my-app:2.0.0'
        sshAgent(['dev-server']){
            sh "ssh -o StrictHostKeyChecking=no ec2-user@public-ip ${dockerRun}"
        }
    }
}
