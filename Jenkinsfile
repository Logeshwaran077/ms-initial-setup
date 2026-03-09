node {
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

        stage('Deploy to GKE') {
            sh '''
                # 1. Authenticate
                gcloud auth activate-service-account --key-file=$GC_KEY
                
                # 2. Get cluster credentials
                gcloud container clusters get-credentials ''' + clusterName + ''' \
                    --zone ''' + zone + ''' \
                    --project ''' + project + '''
                
                # 3. Update the YAML
                sed -i "s|IMAGE_URL|''' + fullRepoPath + '''|g" k8s/deployment.yaml
                
                # 4. Apply
                kubectl apply -f k8s/deployment.yaml
            '''
        } // End of stage 'Deploy to GKE'
        
    } catch (exc) { // This now correctly pairs with 'try'
        echo "Pipeline failed: ${exc.message}"
        throw exc
    }
} // End of node