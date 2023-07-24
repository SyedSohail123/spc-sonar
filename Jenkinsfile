pipeline {
    agent { label 'MAVEN_JDK17' }
    tools {
        jdk 'JDK_17'
        maven 'MAVEN-3.9.3'
    }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('VCS') {
            steps {
                git url: 'https://github.com/SyedSohail123/spc-sonar.git',
                    branch: 'declarative'
            }
        }
        stage('Build & SonarQube Scan') {
            steps {
              withSonarQubeEnv('SONAR_CLOUD') {
                    sh 'mvn clean install sonar:sonar -Dsonar.organisation=qtdevopssohail123 -Dsonar.projectKey=qtdevopssohail123_springpet-clininic'  
               }
            }
        }
        stage('Archiving the artifacts & publishing test results') {
            steps {
                archiveArtifacts onlyIfSuccessful: true,
                            artifacts: '**/target/spring-*.jar'
                junit testResults: '**/surefire-reports/TEST-*.xml' 
            }
        }
    }
}