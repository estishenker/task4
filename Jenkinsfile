pipeline {
    agent any
    environment {
        boolean ifContainerRunning = true
    }
    stages {
        stage('test') {
            steps {
                echo "============test============="
        
            }
        }
        stage('Pull') {
            steps {
                echo 'Pulling latest code ...'                  
                // sh 'cd /home/devops/Documents/'
                git credentialsId: '70f1c332-567d-433c-a4ae-9abb4f725fa8', url: 'https://github.com/estishenker/task4.git/'                    

צריך לעדכן מפתח זה לגישה    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   
                
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
                script {
                    if ( env.ifContainerRunning == true ) {                
                        notifySuccessful()
                    } else {
                        echo "it's not success"
                    }                
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
