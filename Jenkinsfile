pipeline {
    agent { label 'linux' }
    stages {
        stage('Run on Slave') {
            steps {
                script {
                    def dockerImage = 'public.ecr.aws/docker/library/maven:3.9-sapmachine'

                    // Pull the Docker image
                    sh "docker pull ${dockerImage}"

                    // Run Maven and Git commands inside the Docker container
                    sh "docker run --rm -v ${env.WORKSPACE}:/workspace -w /workspace ${dockerImage} mvn --version"
                    sh "docker run --rm -v ${env.WORKSPACE}:/workspace -w /workspace ${dockerImage} git --version"

                    // Continue with your other pipeline steps
                }
            }
        }
        stage('Source') {
            steps {
                sh 'mvn --version'
                sh 'git --version'
                git branch: 'main',
                    url: 'https://github.com/elvislittle/slave-docker-my-git.git'
            }
        }
        stage('Clean') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn clean'
                }
            }
        }
        stage('Test') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn test'
                }
            }
        }
        stage('Package') {
            steps {
                dir("${env.WORKSPACE}/Ch04/04_03-docker-agent"){
                    sh 'mvn package -DskipTests'
                }
            }
        }
    }
}
