pipeline {
    agent any
    environment {
        boolean ifContainerRunning = true
    }
    stages {
        stage('Pull') {
            steps {
                echo 'Pulling latest code ...'                              
                git credentialsId: 'credentials for task4 project', url: 'https://github.com/estishenker/task4.git'
                sh 'cd /home/devops/Documents/'
                sh 'if [ -d "nodeapp" ]; then rm -R nodeapp; fi'
                sh 'git clone "https://github.com/estishenker/task4.git" /home/devops/Documents/nodeapp/' 
            }
        }
        stage('Build my_node_app image') {
            steps {
                echo 'Building update image ...'
                sh 'docker build .'
            }
        }        
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
                  ifContainerRunning == false
                '''
            }
        }
        stage('Notification with URL') {
            steps {
                if( ifContainerRunning == true ){                
                    notifySuccessful()
                }
            }
        }    
    }
}
def notifySuccessful() {  
    emailext subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
        body: """<p>SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
        recipientProviders: [[$class: 'DevelopersRecipientProvider']],
        to: 'estishenker@gmail.com'
}
