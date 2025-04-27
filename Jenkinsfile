pipeline {
    agent {
        label 'master'
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
        stage('Deploy') {
            steps {
                sh '''
                scp target/java-webapp-1.0.jar deploy@localhost:~/staging/java-webapp-1.0.jar && \
                ssh deploy@localhost "java -jar ~/staging/java-webapp-1.0.jar 2> /dev/null &"
                '''
            }
        }
    }
}
