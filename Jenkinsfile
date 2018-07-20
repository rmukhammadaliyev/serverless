pipeline {
    agent  none

    environment {
    	FUNC_NAME='hello'
    }
    stages {
        stage('Stage') {
            agent {
                   label 'master'
            }
            steps {
              withAWS(credentials:'AWS-LAMBDA-USER',region:'us-east-1') {
                sh '''
                  cd myService
                  AWSREGION="us-east-1"
                  ALIAS=stage
                  sls remove -v
               '''
            }
        }
    }
  }
}