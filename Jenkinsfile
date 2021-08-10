pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true'           
            }
        }
        stage ('Unit Tests') {
            steps {
                  sh 'mvn test'              
            }
        }  
          stage ('Functional Test') {
            steps {
                dir('functional-test') {
                    git credentialsId: 'github_login', url: 'https://github.com/amsfrancine/tasks-functional-tests-1.git'
                    sh 'mvn test'
                }
            }
        }           
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {             
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=663743b232e26a55628b2b67eb84877379500f8c -Dsonar.java.binaries=target"
                }   
            }
        } 
         stage ('Quality Gate') {
            steps {
                sleep(5)
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}  
