node {
    def mvnHome = tool 'maven-3.5.2'  // Make sure this tool is configured in Jenkins
    def dockerImageTag = "vickeyyvickey/myapplication:${env.BUILD_NUMBER}"

    stage('Clone Repo') {
        git 'https://github.com/vikas4cloud/CI-CD-java-maven-docker.git'
    }

    stage('Build Project') {
        sh "${mvnHome}/bin/mvn clean install"
    }

    stage('Build Docker Image') {
        // Force rebuild Docker image with no cache
        sh "docker build --no-cache -t ${dockerImageTag} ."
    }

    stage('Docker Login & Push') {
        // Replace with Jenkins credentials for secure login
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh 'echo "$DOCKER_PASS" | docker login -u rajv690 -p Rocky69023'
        }

        // Push with versioned tag
        sh "docker push ${dockerImageTag}"

        // Also push as 'latest' if needed
        sh "docker tag ${dockerImageTag} rajv690/myapplication:latest"
        sh "docker push rajv690/myapplication:latest"
    }
}
