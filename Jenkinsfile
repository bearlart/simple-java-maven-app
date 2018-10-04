pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
            steps {
                agent {docker}
                sh 'run --name tomcat -d  -p 8080:8080 \
-v /ice/docker/ProjetoJava/IRRFWeb/target/IRRFWeb-1.0-SNAPSHOT.war:/usr/local/tomcat/webapps/IRRFWeb-1.0-SNAPSHOT.war tomcat'
            }
        }
    }
}
