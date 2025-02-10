pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"  // Set Java home if required
        MAVEN_HOME = "/usr/share/maven"
        TOMCAT_USER = "admin"  // Tomcat manager username
        TOMCAT_PASS = "admin"  // Tomcat manager password
//        TOMCAT_URL = "http://3.87.79.249:9090/"  // Change this to your Tomcat server IP
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/amitlondhe7557/SampleWebApp.git'
            }
        }

        stage('Build and Package') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                def warFile = "target/SampleWebApp.war"
                def tomcatUrl = "http://54.175.156.210:8080"

            echo "Deploying ${warFile} to Tomcat at ${tomcatUrl}"

            // Use curl to deploy WAR via Tomcat Manager API
            sh """
                curl -v --user ${TOMCAT_USER}:${TOMCAT_PASS} \
                --upload-file ${warFile} \
                "${tomcatUrl}/manager/text/deploy?path=/SampleWebApp&update=true"
            """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo "Deployment failed. Check logs!"
        }
    }
}
