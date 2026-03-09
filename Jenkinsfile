node {
    // 1. Define Tools and Variables
    def mvnHome = tool name: 'maven', type: 'maven'
    def mvnCMD = "${mvnHome}/bin/mvn"
    
    try {
        stage('Cleanup') {
            cleanWs() 
        }

        stage('Checkout') {
            checkout([
                $class: 'GitSCM', 
                branches: [[name: '*/main']], 
                userRemoteConfigs: [[
                    url: 'https://github.com/Logeshwaran077/ms-initial-setup.git',
                    credentialsId: 'git' 
                ]]
            ])
        }

        stage('Build') {
            // This runs your Maven build (compiling and testing)
            sh "${mvnCMD} clean install"
        }
        
    } catch (exc) {
        echo "Pipeline failed: ${exc.message}"
        throw exc
    }
}