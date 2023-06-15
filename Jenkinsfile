pipeline {
    agent any
    stages {
        stage('Pull') {
            steps {
                script{
                    echo 'Pulling latest code ...'                    
                    git clone 'https://github.com/estishenker/task4.git' /home/devops/Documents/nodeapp/
                }
            }
        }
        stage('Build my_node_app image') {
            steps {
                echo 'Building update image ...'
                sh 'docker build .'
            }
        }
        boolean ifContainerRunning = true
        stage('Test') {
            steps {
                echo 'Testing if image created ...'
                sh ''' 
                if [ docker images |grep my_node_app ]
                then
                  echo "image ${docker images |grep my_node_app) created"
                  docker run my_node_app          
                else
                  echo "failure"
                  ifContainerRunning = false
                '''
            }
        }
        stage('Notification with URL') {
            if(ifContainerRunning){           
                notifySuccessful()            
            }
        }
        def notifySuccessful() {  
            emailext (
                subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']]
                )
        }
    }
}
