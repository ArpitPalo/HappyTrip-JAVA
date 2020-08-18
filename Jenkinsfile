pipeline {
    agent any
    parameters {
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Do you want to deploy')
        booleanParam(name: 'CODE_CHECK', defaultValue: false, description: 'Do you want to deploy')
    }
    stages {
        stage('Build & Sonar') {
            tools {
                jdk 'jdk8'
            }
            when{
                expression { params.CODE_CHECK == true }
            }
            steps {
                echo "Build Project & Using SonarQube"
                withSonarQubeEnv('Sonar') {
                        powershell label: '', script: 'mvn package -f happytrip-code\\pom.xml sonar:sonar'
                }
                
            }
        }
        stage('Build'){
            tools {
                jdk 'jdk8'
            }
            when{
                expression { params.CODE_CHECK == false }
            }
            steps {
                echo "Build Project"
                powershell label: '', script: 'mvn package -f happytrip-code\\pom.xml'
                
            }
        }
        stage('Deploy'){
            when{
                    expression { params.DEPLOY == true }
            }
            steps {
                    deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8081/')], contextPath: 'happytrip-new', onFailure: false, war: '**/*.war'
            }
        }
    }
    post {
        always {
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
                
                //deploy adapters: [tomcat7(credentialsId: 'tomcat7', path: '', url: 'http://localhost:8081/')], contextPath: 'happytrip-new', onFailure: false, war: '**/*.war'
                
        }
    }
}
