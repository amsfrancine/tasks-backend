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
                    git credentialsId: 'https://github.com/amsfrancine/tasks-functional-tests-1.git'
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
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=fa9f3729c25471dedd0c7a3177b5ebf7ad195e6d -Dsonar.java.binaries=target"
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
