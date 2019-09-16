pipeline {
  agent {
    label 'mule-builder'
  }  
  environment {
    ENV = 'test'
    ENVIRONMENT = 'Sandbox'
  }
  
  stages {
    stage('Build') {
      steps {
        withMaven(){
            sh 'mvn clean package'
          }
      }
    }

    stage('Test') {
      steps {
        withMaven(){
            sh "mvn -B test"
        }
      }
    }

    stage('Deploy') {
      environment {
        APP_NAME = 'salesforce-crm-app'
        APP_CLIENT_CREDS = credentials("$ENV-app-client-creds")
        ANYPOINT_ENV = credentials("$ENV-anypoint-creds")
        BG = "Test"
        WORKER = "MICRO"
      }
      
      steps {
        withMaven(){
            sh 'echo "Building environemnt: $ENV"'
            sh 'mvn -V -B deploy -DmuleDeploy \
              -Denv=$ENV \
              -Dmule.version=$MULE_VERSION \
              -Danypoint.username=$DEPLOY_CREDS_USR \
              -Danypoint.password=$DEPLOY_CREDS_PSW \
              -Dcloudhub.app=$APP_NAME \
              -Dcloudhub.environment=$ENVIRONMENT \
              -Denv.ANYPOINT_CLIENT_ID=$ANYPOINT_ENV_USR \
              -Denv.ANYPOINT_CLIENT_SECRET=$ANYPOINT_ENV_PSW \
              -Dcloudhub.bg=$BG \
              -Dcloudhub.worker=$WORKER \
              -Dapp.client_id=$APP_CLIENT_CREDS_USR \
              -Dapp.client_secret=$APP_CLIENT_CREDS_PSW'
          }
      }
    }
  }

  tools {
    maven 'M3'
  }
}