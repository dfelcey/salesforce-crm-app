pipeline {
  agent {
    label 'mule-builder'
  }
  
  environment {
    DEPLOY_CREDS = credentials('anypoint-user')
    MULE_VERSION = '4.2.0'
  }
  
  stages {
    stage('Prepare') {
      steps {
        configFileProvider(
          [configFile(fileId: 'config-file', variable: 'CONFIG_FILE')]) {
            sh "cp \$CONFIG_FILE ./src/main/resources/config.properties"
        }
      }
    }
    
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

    stage('Deploy Sandbox') {
      environment {
        ENVIRONMENT = 'Sandbox'
        APP_NAME = 'salesforce-crm-app'
        APP_CLIENT_CREDS = credentials('app-client-creds')
        BG = "Test"
        WORKER = "MICRO"
      }
      
      steps {
        withMaven(){
            sh 'mvn -V -B deploy -DmuleDeploy -Dmule.version=$MULE_VERSION -Danypoint.username=$DEPLOY_CREDS_USR -Danypoint.password=$DEPLOY_CREDS_PSW -Dcloudhub.app=$APP_NAME -Dcloudhub.environment=$ENVIRONMENT -Denv.ANYPOINT_CLIENT_ID=$ANYPOINT_ENV_USR -Denv.ANYPOINT_CLIENT_SECRET=$ANYPOINT_ENV_PSW -Dcloudhub.bg=$BG -Dcloudhub.worker=$WORKER -Dapp.client_id=$APP_CLIENT_CREDS_USR -Dapp.client_secret=$APP_CLIENT_CREDS_PSW'
          }
      }
    }
  }

  tools {
    maven 'M3'
  }
}