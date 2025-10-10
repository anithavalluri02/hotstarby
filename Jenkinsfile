pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from main branch
                git branch: 'main', url: 'https://github.com/anithavalluri02/hotstarby.git'

                // Verify files
                sh 'pwd'
                sh 'ls -l'
                sh 'ls -R'
            }
        }
        stage('upload to nexus') {
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'myapp', classifier: '', file: 'target/myapp.war', type: 'war']], credentialsId: 'nexus-cred', groupId: 'in.reyaz', nexusUrl: '3.108.222.90:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: '8.3.3'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker rmi -f hotstar:v1 || true
                    docker build -t hotstar:v1 -f /var/lib/jenkins/workspace/hotstar/Dockerfile /var/lib/jenkins/workspace/hotstar
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f con8 || true
                    docker run -d --name con8 -p 8008:8080 hotstar:v1
                '''
            }
        }

    }
}
