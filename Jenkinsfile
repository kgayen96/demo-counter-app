pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                git branch: 'main', url: 'https://github.com/kgayen96/demo-counter-app.git'
            }
        }
        stage('UNIT testing'){
            
            steps{
                sh 'mvn test'
            }
        }
        stage('Interation testing'){
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }    
    }        
}
