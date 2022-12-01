node{
      
    stage('Git Checkout'){
          git branch: 'main', url: 'https://github.com/kgayen96/demo-counter-app.git'
    }
        stage('Maven Build'){
            def mavenHome = tool name: "Maven-3.8.6", type: "maven"
            def mavenCMD = "${mavenHome}/opt/mvn"
            sh "${mavenCMD} clean package"
    }
}
