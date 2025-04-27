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
                -Dsonar.token=sqp_d51481e50359e5f13d88beaea5815ca341524e5b
                '''
            }
        }
    }
}
