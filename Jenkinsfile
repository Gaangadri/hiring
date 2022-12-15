pipeline {
    agent any
    stages {
        stage("git-checkout") {
            steps {
                git branch: 'main', credentialsId: 'git-crds', url: 'https://github.com/Gaangadri/hiring'
            }
        }
        stage("maven-build") {
            steps {
                sh 'mvn clean package'
            }
        }
        stage("tomcat-deploy") {
            steps {
                sshagent(['tomcat9']) {
                    sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.91.20:/opt/tomcat09/webapps"
                    sh "ssh ec2-user@172.31.91.20 /opt/tomcat09/bin/shutdown.sh"
                    sh "ssh ec2-user@172.31.91.20 /opt/tomcat09/bin/startup.sh"
               }
            }
        }
    }
}
