node {
    def mvnHome = tool 'maven-3.5.2' // Ensure this is configured in Jenkins
    def dockerImageTag = "rajv690/myapplication:${env.BUILD_NUMBER}"

    stage('Clone Repository') {
        git 'https://github.com/vikas4cloud/CI-CD-java-maven-docker.git'
    }

    stage('Build Maven Project') {
        sh "${mvnHome}/bin/mvn clean install"
    }

    stage('Build Docker Image') {
        // Rebuild Docker image with no cache to ensure latest code is included
        sh "docker build --no-cache -t ${dockerImageTag} ."
    }

    stage('Docker Login') {
        // WARNING: Hardcoded credentials, okay for learning/testing only
        sh 'docker login -u rajv690 -p Rocky69023'
    }

    stage('Push Docker Image') {
        // Push versioned image
        sh "docker push ${dockerImageTag}"

        // Also push 'latest' tag (optional but common)
        sh "docker tag ${dockerImageTag} rajv690/myapplication:latest"
        sh "docker push rajv690/myapplication:latest"
    }

    stage('Cleanup') {
        echo "Docker image ${dockerImageTag} successfully built and pushed."
    }
}
