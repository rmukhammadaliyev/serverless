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
                  sls deploy -v
                  #Get Version after publishing
                  VERSION=$(aws lambda publish-version --function-name $FUNC_NAME --region $AWSREGION | jq '.Version')
                  eval VERSION=$VERSION
                  LISTEDALIAS=$(aws lambda list-aliases --function-name $FUNC_NAME --region $AWSREGION | jq '.Aliases[].Name')
                  eval LISTEDALIAS=$LISTEDALIAS
                  #Check if alias exists, then update. If not create alias
                  if [ $ALIAS == $LISTEDALIAS ]
                      then
                        aws lambda update-alias --region $AWSREGION --function-name $FUNC_NAME --function-version $VERSION --name $ALIAS
                   else
                        aws lambda create-alias --region $AWSREGION --function-name $FUNC_NAME --function-version $VERSION --name $ALIAS
                   fi
               '''
            }
        }
    }
  }
}