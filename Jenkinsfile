pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                git branch: 'main', url: 'https://github.com/kgayen96/demo-counter-app.git'
            }
        }
        stage('Maven Build'){
            
            steps{
                sh 'mvn clean package'
            }
        }
    }
}
