pipeline {
    agent any
    stages {
        stage('Build') {
            tools {
                jdk 'jdk8'
            }
            steps {
                echo "Build Project"
                withSonarQubeEnv('Sonar') {
                    powershell label: '', script: 'mvn package -f happytrip-code\\pom.xml sonar:sonar'
                }
                
            }
        }
    }
    post {
        always {
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8081/')], contextPath: 'happytrip-new', onFailure: false, war: '**/*.war'
        }
    }
}
