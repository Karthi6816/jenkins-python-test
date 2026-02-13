pipeline {
    agent { label 'Jenkins-node' }

    triggers {
        pollSCM('*/5 * * * 1-5')
    }

    options {
        skipDefaultCheckout(true)
        // Keep the 10 most recent builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timestamps()
    }

    environment {
      PATH = "/home/ec2-user/anaconda2/bin:$PATH"
    }

    stages {
 
        stage ("Code pull"){
            steps{
                checkout scm
            }
        }

        stage('Build environment') {
            steps {
                echo "Building virtualenv"
                sh  ''' export PATH=/home/ec2-user/anaconda2/bin:$PATH
                        pip install -r requirements/dev.txt
                    '''
            }
        }

    }

    post {
        always {
            sh 'conda remove --yes -n ${BUILD_TAG} --all'
        }
        failure {
            emailext (
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: """<p>FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
                         <p>Check console output at &QUOT;<a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a>&QUOT;</p>""",
            )

        }
    }

}












