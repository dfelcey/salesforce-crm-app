pipeline {
  agent {
    label 'mule-builder'
  }  
  
  environment {
  	ENV_NAME = 'test'   
    ANYPOINT_CREDS = credentials("$ENV_NAME-anypoint-creds")
	APP_NAME = 'salesforce-crm-app'
	ANYPOINT_ENV = 'Sandbox'
	ANYPOINT_BG = 'Test'
	ANYPOINT_WORKERS = 1
	MULE_VERSION = '4.2.0' 	
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
            sh 'mvn -Denv=$ENV_NAME clean package'
          }
      }
    }

    stage('Test') {
      steps {
        withMaven(){
            sh "mvn -B -Denv=$ENV_NAME test"
        }
      }
    }

    stage('Deploy') {
      environment {
        // For CRM access
        CRM_CREDS = credentials("$ENV_NAME-crm-creds")
        CRM_TOKEN = credentials("$ENV_NAME-crm-token")
        CRM_URL = credentials("$ENV_NAME-crm-url")
      }
      
      steps {
        withMaven(){
            sh 'echo "Deploying environment for: $ENV_NAME"'
            sh 'mvn  \
              -Dsfdc.username=$CRM_CREDS_USR \
              -Dsfdc.password=$CRM_CREDS_PWD \
              -Dsfdc.token=$CRM_TOKEN \
              -Dsfdc.url=$CRM_URL \
              -Denv=$ENV_NAME \
              -V -B deploy -DmuleDeploy'
           }
      }
    }
  }
}