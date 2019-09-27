pipeline {
  agent {
    label 'mule-builder'
  }  
  
  environment {
  	ENV_NAME = 'test'   
    ANYPOINT_CREDS = credentials("$ENV_NAME-anypoint-creds")
    ANYPOINT_CLIENT_CREDS = credentials("$ENV_NAME-anypoint-client-creds")
	APP_NAME = 'salesforce-crm-app-$ENV_NAME'
	ANYPOINT_ENV = 'Sandbox'
	ANYPOINT_BG = 'Test'
	
    // For CRM access
    CRM_CREDS = credentials("$ENV_NAME-crm-creds")
    CRM_TOKEN = credentials("$ENV_NAME-crm-token")
    CRM_URL = credentials("$ENV_NAME-crm-url")	
    
    MVN_SYS_PROPS = """
    	-Dmule.env=$ENV_NAME \
    	-Dapp.name=$APP_NAME \
    	-Danypoint.username=$ANYPOINT_CREDS_USR \
    	-Danypont.password=$ANYPOINT_CREDS_PWD \
    	-Danypoint.environment=$ANYPOINT_ENV \
    	-Danypont.business_group=$ANYPOINT_BG \
    	-Dnypoint.platform.client_id=$ANYPOINT_CLIENT_CREDS_USR \
    	-Danypoint.platform.client_secret=$ANYPOINT_CLIENT_CREDS_PWD \
    	-Dsfdc.username=$CRM_CREDS_USR \
    	-Dsfdc.password=$CRM_CREDS_PWD \
    	-Dsfdc.token=$CRM_TOKEN -Dsfdc.url=$CRM_URL
    	"""
  }
  
  triggers {
    pollSCM('* * * * *')
  }

  tools {
    maven 'M3'
  }
  
  stages {
    stage('Build') {
      steps {
        withMaven(){
            sh 'echo "Building environment for: $ENV"'
            sh 'mvn -V $MVN_SYS_PROPS clean package'
          }
      }
    }

    stage('Test') {
      steps {
        withMaven(){
            sh 'env'
            sh 'mvn -V -B $MVN_SYS_PROPS test'
        }
      }
    }

    stage('Deploy') {
      steps {
        withMaven(){
            sh 'echo "Deploying environment for: $ENV_NAME"'
            sh 'mvn -V -B $MVN_SYS_PROPS deploy -DmuleDeploy'
           }
      }
    }
  }
}