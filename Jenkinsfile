pipeline {
    agent { label 'dockeragent'}
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                git url: 'https://github.com/ravikiran2596/spring-petclinic.git',
                   branch: 'main'
            }
        }
        stage('build') {
            tools {
                jdk ('jdk_17')
            }
            steps {
                sh 'mvn package'
            }
        }
        stage('sonar analysis') {
            steps {
                ps {
                // performing sonarqube analysis with "withSonarQubeENV(<Name of Server configured in Jenkins>)"
                withSonarQubeEnv('sonar_cloud') {
                    sh 'mvn clean package sonar:sonar -Dsonar.organization=springpetclinic1'
                }
            }
        }
        stage('archiveartifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
            }
        }
        stage('testresults') {
            steps {
                junit testResults: '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}