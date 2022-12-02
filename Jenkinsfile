node{
      
    stage('Git Checkout'){
          git branch: 'main', url: 'https://github.com/kgayen96/demo-counter-app.git'
    }
    
      stage('Maven Build'){
             
            sh 'mvn test'
      }      
}
