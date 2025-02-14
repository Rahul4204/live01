pipeline {
    agent {
        label 'dev'
    }

    stages {
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/Rahul4204/live01.git'
            }
        }
        stage('maven') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('sonar') {
            steps {
                withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonarpipeline') {
                    sh 'mvn clean verify sonar:sonar'
                }
            }
        }
         stage('nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'vamsi', classifier: '', file: 'target/live.war', type: 'war']], credentialsId: 'nexus', groupId: 'vamsi.maven.com', nexusUrl: '13.233.142.101:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-releases', version: '2.0'
            }
         }
          stage('tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://15.207.99.211:8080')], contextPath: 'live1', war: '**/*.war'
            }
        
        }    
    }
}
