pipeline {
  agent {
    label 'mule-builder'
  }  
  
  environment {
  	ENV_NAME = 'test'   
    ANYPOINT_CREDS = credentials("$ENV_NAME-anypoint-creds")
    ANYPOINT_CLIENT_CREDS = credentials("$ENV_NAME-anypoint-client-creds")
	APP_NAME = 'salesforce-crm-app'
	ANYPOINT_ENV = 'Sandbox'
	ANYPOINT_BG = 'Test'
	ANYPOINT_WORKERS = 1
	MULE_VERSION = '4.2.0' 	
	
    // For CRM access
    CRM_CREDS = credentials("$ENV_NAME-crm-creds")
    CRM_TOKEN = credentials("$ENV_NAME-crm-token")
    CRM_URL = credentials("$ENV_NAME-crm-url")	
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
        		sh 'env'
            sh 'mvn -V -Denv=$ENV_NAME clean package'
          }
      }
    }

    stage('Test') {
      steps {
        withMaven(){
            sh 'env'
            sh 'mvn -V -B -Denv=$ENV_NAME test'
        }
      }
    }

    stage('Deploy') {
      steps {
        withMaven(){
            sh 'echo "URL is $CRM_URL"'
         	sh 'env'
            sh 'echo "Deploying environment for: $ENV_NAME"'
            sh 'mvn -V -B deploy -DmuleDeploy'
           }
      }
    }
  }
}