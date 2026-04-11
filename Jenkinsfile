pipeline {
    agent any
    stages {
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(
                    credentialsId: 'sonar-token',
                    variable: 'SONAR_TOKEN')]) {
                    sh """
                        mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=docker \
                        -Dsonar.projectName=docker \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=\${SONAR_TOKEN}
                    """
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t zakariazammouri/docker:latest .'
            }
        }
        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHub',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS')]) {
                    sh 'docker login -u $USER -p $PASS'
                    sh 'docker push zakariazammouri/docker:latest'
                }
            }
        }
    }
}
