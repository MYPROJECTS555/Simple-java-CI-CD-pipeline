pipeline{
    agent any
    
    tools{
        jdk 'java-11'
        maven 'maven'
    }
    
    stages{
        stage('Git-checkout'){
            steps{
                git branch: 'main' , url: 'https://github.com/MYPROJECTS555/Simple-java-CI-CD-pipeline.git'
            }
        }
        stage('Code Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Code Package'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build and tag'){
            steps{
                sh 'docker build -t nameisvikas/javaproject .'
            }
        }
        stage('trivy'){
            steps{
                sh 'trivy image --format table -o trivy_report.html nameisvikas/javaproject'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name container-java -p 9085:8080 nameisvikas/javaproject
                '''
            }
        }
        stage('Login to Docker Hub') {
                    steps {
                        script {
                            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                                sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                            }
                        }
                    }
        }
         stage('Pushing image to repository'){
            steps{
                sh 'docker push nameisvikas/javaproject'
            }
        }
        
    }
}
