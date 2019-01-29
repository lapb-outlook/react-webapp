pipeline {
    agent {
        node {
            label 'agent1'
        }
    }
    stages {
        def packageName = "release.${BUILD_ID}.tar.gz"

        stage('Build'){
            steps {            
                sh "yarn install"
                sh "yarn build"
            }
        }

        stage('Test'){
            steps {            
                sh "pwd"
                sh "cd build"                
                sh "http-server -p 9000 . > /dev/null 2>&1 &"
                sh "sleep 5"
                sh "curl -s -I http://localhost:9000/"
                sh "yarn test --detectOpenHandles"
                sh "echo done!!!"        
            }
        }

        stage('Package'){
            steps {                
                sh "pwd"
                sh "ls -la"
                sh "tar -czvf ${packageName} -C build/ ."
            }
        }        
    }

    post {
        always {
            archiveArtifacts artifacts: packageName, fingerprint: true
            publishHTML (target: [
                allowMissing: false,
                alwaysLinkToLastBuild: false,
                keepAll: true,
                reportDir: 'output/coverage/jest',
                reportFiles: 'index.html',
                reportName: 'Test Report'
            ])
        }
    }    
}
