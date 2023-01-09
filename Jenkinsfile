pipeline{
    
    agent any
    
    stages {
        
        stage('Git checkout'){
            
            steps{
                
                script{
                    
                    git branch: 'main', url: 'https://github.com/kgayen96/demo-counter-app.git'
                }
            }
        }
        stage('UNITtesting'){
            
            steps{
                
                script{
                    
                    sh 'mvn test'
                }
            }
        }
        stage('Integration testing'){
            
            steps{
                
                script{
                    
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
        stage('Maven build'){
            
            steps{
                
                script{
                    
                    sh 'mvn clean install'
                }
            }
        }
        stage('Static code analysis'){
            
            steps{
                
                script{
                    
                    withSonarQubeEnv(credentialsId: 'sonar-api') {
                        
                        sh 'mvn clean package sonar:sonar'
                    }
                   }
                }
            }
            stage('Quality Gate Status'){
                
                steps{
                    
                    script{
                        
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api'
                    }
                }
            }
            stage('nexus artifact uploader'){

                steps{

                    script{
                        def readPomVersion = readMavenPom file: 'pom.xml'
                        
                        def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "javaproject-snapshot" : "javaproject-release"

                        nexusArtifactUploader artifacts: 
                        [
                            [
                                artifactId: 'springboot', 
                                classifier: '', 
                                file: 'target/Uber.jar', 
                                type: 'jar'
                                ]
                        ], 
                         credentialsId: 'nexus-creds', 
                         groupId: 'com.example', 
                         nexusUrl: '15.207.110.202:8081', 
                         nexusVersion: 'nexus3', 
                         protocol: 'http', 
                         repository: nexusRepo, 
                         version: "${readPomVersion.version}"
                    }
                }
            }
            stage('docker image building'){
                
                steps{
                    
                    script{
                        
                        sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID kg95/$JOB_NAME:v1.$BUILD_ID'
                        sh 'docker image tag $JOB_NAME:v1.$BUILD_ID kg95/$JOB_NAME:latest' 
                    }
                }
            }
            stage('Push Image to DockerHub'){
                
                steps{
                    
                    script{
                        
                        withCredentials([string(credentialsId: 'dockerHub_password', variable: 'dockerHub_password')]) {
                            
                            sh 'docker login -u kg95 -p ${dockerHub_password}'
                            sh 'docker image push kg95/$JOB_NAME:v1.$BUILD_ID'
                            sh 'docker image push kg95/$JOB_NAME:latest'
                      }
                    }
                }
            }
        }
}
