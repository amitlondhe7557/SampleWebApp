pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64"  
        MAVEN_HOME = "/usr/share/maven"
        TOMCAT_USER = "admin"  
        TOMCAT_PASS = "admin"  
        TOMCAT_URL = "http://3.87.79.249:9090"
        WAR_FILE = "target/SampleWebApp.war"
        CONTEXT_PATH = "/SampleWebApp"
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

        stage('Undeploy Previous Version') {
            steps {
                script {
                    echo "Undeploying previous application from ${TOMCAT_URL}"
                    sh """
                        curl -v --user ${TOMCAT_USER}:${TOMCAT_PASS} \
                        "${TOMCAT_URL}/manager/text/undeploy?path=${CONTEXT_PATH}"
                    """
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    echo "Deploying ${WAR_FILE} to Tomcat at ${TOMCAT_URL}"
                    sh """
                        curl -v --user ${TOMCAT_USER}:${TOMCAT_PASS} \
                        --upload-file ${WAR_FILE} \
                        "${TOMCAT_URL}/manager/text/deploy?path=${CONTEXT_PATH}"
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
