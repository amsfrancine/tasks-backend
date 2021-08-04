pipeline {
    agent any
    stages {
        stage('Build Backend') {
            steps {
                sh 'mvn clean package -DskipTests=true'           
            }
        }
        stage('Stage 1') {
            agent none
            steps {
                    script {
                        Logs = input id: 'test', message: 'Select:', ok: 'Proceed?', parameters: [choice(choices: 'sonar\nflowhr-front-dev\nflowhr-front-hlg\nflowhr-front-prd\npessoas-back-dev\npessoas-back-hlg\npessoas-back-prd\ncompetencias-back-dev\ncompetencias-back-hlg\ncompetencias-back-prd', name: 'myparam')], submitterParameter: 'APPROVER'
                    }
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
                    sh "${scannerHome}/bin/sonar-scanner -e -Dsonar.projectKey=DeployBack -Dsonar.host.url=http://localhost:9000 -Dsonar.login=779c3dc8d2d973a7fd905db7904959bacc1dc174 -Dsonar.java.binaries=target"
                }   
            }
        }
    }
}  
