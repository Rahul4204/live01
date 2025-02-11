pipeline {
    agent any

    environment {
        SONARQUBE_URL = 'http://172.31.45.10:9000/'  
        NEXUS_URL = 'http://172.31.42.161:8081/'      
        TOMCAT_URL = 'http://3.110.194.196:8080/'     
        PROJECT_NAME = 'live01'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Rahul4204/live01.git'
            }
        }

        stage('Code Quality Check') {
            steps {
                script {
                    sh 'sonar-scanner -Dsonar.projectKey=live01 -Dsonar.host.url=$SONARQUBE_URL'
                }
            }
        }

        stage('Build Artifact') {
            steps {
                sh 'mvn clean install'
            }
        }
   stage('Upload to Nexus') {
    steps {
        nexusArtifactUploader(
            nexusVersion: 'nexus3',
            protocol: 'http',
            nexusUrl: "$NEXUS_URL",
            repository: 'rahul-repo',
            credentialsId: 'nexus-credentials',
            groupId: 'com.example',
            version: '1.0.0',
            artifacts: [
                [
                    artifactId: 'live01',
                    classifier: '',
                    file: 'target/live01.war',
                    type: 'war'
                ]
            ]
        )
    }
}



        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: "$TOMCAT_URL")], war: 'target/live01.war'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
