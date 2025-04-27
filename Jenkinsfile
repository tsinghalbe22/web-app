pipeline {
    agent {
        label 'agent'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
//         stage('Sonar-Report') {
//             steps {
//             sh 'mvn sonar:sonar \
//   -Dsonar.projectKey=jenkins_project \
//   -Dsonar.host.url=http://localhost:9000 \
//   -Dsonar.login=5f09ded7e5db4d0ea0dcfd937c181af706e60475'
//             }
//         }
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
        stage('Sonar-Report') {
            steps {
                sh '''
                mvn clean verify sonar:sonar \
                -Dsonar.projectKey=web-app \
                -Dsonar.projectName='web-app' \
                -Dsonar.host.url=http://localhost:9000 \
                -Dsonar.token=sqp_de52cc5e9ea5fe3ae085fe7d3577ae5e677367b6
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                scp target/java-webapp-1.0.jar deploy@localhost:~/staging/java-webapp-1.0.jar && \
                ssh deploy@localhost "ssh deploy@localhost "nohup java -jar ~/staging/java-webapp-1.0.1.jar & echo \$! > ~/staging/java-webapp.pid""
                '''

                sh 'sleep 600'
                    
                sh '''
                ssh deploy@localhost "kill -9 \$(cat ~/staging/java-webapp.pid)"
                '''

            }
        }
    }
}
