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
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'SONAR_SCANNER'
            }
            steps {
                withSonarQubeEnv('SONAR_LOCAL') {             
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=e67f9a538da578ba09d776b5a8b07d448c68456a -Dsonar.java.binaries=target"
                }   
            }
        }
    }
}  
