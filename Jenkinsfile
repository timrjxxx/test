pipeline{
    agent any

    stages{

        stage("pull"){

            steps{
                checkout scm
            }
        }

        stage("test run"){
            steps{
                bat './mvnw clean test'
            }
            post{
                always{
                    junit '**/target/surefire-reports/TEST-*xml'
                }
            }
        }
         stage("build"){
            steps{
                  bat ' ./mvnw  clean compile'
            }
         }
        stage("Scan"){
            steps{
                withSonarQubeEnv(installationName: 'sonar' ){
                  bat  'mvn clean verify sonar:sonar -Dsonar.projectKey=cicd  '
                }
            }
        }
        stage("Jacoco"){
            steps{
                jacoco()
            }
        }
        stage("package"){
            steps{
                 bat './mvnw clean package'
            }
            post{
                success{
                    archiveArtifacts 'target/*.war'
                }

            }
        }
        stage('Deploy'){
             steps {
                    deploy adapters: [tomcat9( url : 'http://localhost:8080/' ,  credentialsId: 'tomcat-cred', path: '/manager/text' )],
                    war: 'target/*.war'
             }
        }

    }



}