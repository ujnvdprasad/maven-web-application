pipeline {
    agent{
        label 'slave'
    }
    tools {
        maven "Maven-3.9.6"
    }
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')
    }
    triggers {
        cron '* * * * *'
        pollSCM '* * * * *'
    }
    stages {
        stage('Checkout Code from GitHub') {
            steps{
                git 'https://github.com/ujnvdprasad/maven-web-application.git'
            }
        }
        stage('Build and Package') {
            steps{
                sh "mvn clean package"
            }
        }
        stage('SonarQube Code Analysis') {
            steps{
                sh "mvn sonar:sonar"
            }
        }
        stage('Upload to Artifact') {
            steps{
                sh "mvn deploy"
            }
        }
        stage('Deploy Artifact') {
            steps{
                sshagent(['Tomcat-Ssh-Connection']) {
                    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@3.108.60.156:/opt/apache-tomcat-9.0.84/webapps"
                }
            }
        }
    }
}
