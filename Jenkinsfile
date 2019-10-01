pipeline {
  agent {
    label 'mule-builder'
  }
  
  environment {  
  	ENV_NAME = 'test'   
    DEPLOY_CREDS = credentials('deploy-anypoint-user')
    MULE_VERSION = '4.2.1'
    	APP_NAME = "salesforce-crm-app-$ENV_NAME"
    BG = "EMEA UK"
  }
  
  stages {
    stage('Prepare') {
      steps {
        configFileProvider([configFile(fileId: "${APP_NAME}-config.properties", replaceTokens: true, targetLocation: './src/main/resources/${ENV_NAME}-config.properties')]) {
          sh 'echo "Branch NAME: $BRANCH_NAME"'
          sh 'echo "Environment NAME: $ENV_NAME"'
        }
      }
    }
    
    stage('Build') {
      steps {
        sh 'mvn -B clean package -Dmule.env=$ENV_NAME -Dmule.version=$MULE_VERSION -DskipTests'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -B test -Dmule.env=$ENV_NAME -Dmule.version=$MULE_VERSION'
      }
    }
    	
    stage('Deploy Development') {
      when {
        branch 'develop'
      }
      environment {
        ENVIRONMENT = 'Sandbox'
        ANYPOINT_CLIENT_CREDS = credentials("$ENV_NAME-anypoint-client-creds")
      }
      steps {
        sh 'mvn -V -B -DskipTests deploy -DmuleDeploy -Dmule.env=$ENV_NAME -Dmule.version=$MULE_VERSION -Danypoint.username=$DEPLOY_CREDS_USR -Danypoint.password=$DEPLOY_CREDS_PSW -Dnypoint.platform.client_id=$ANYPOINT_CLIENT_CREDS_USR -Danypoint.platform.client_secret=$ANYPOINT_CLIENT_CREDS_PWD -Dapp.name=$APP_NAME -Danypoint.environment=$ENVIRONMENT -Danypont.business_group=$BG '
      }
    }
  }

  tools {
    maven 'M3'
  }
}