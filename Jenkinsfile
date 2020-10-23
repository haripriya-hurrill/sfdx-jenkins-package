#!groovy

import groovy.json.JsonSlurperClassic

node {
    try{
        echo "define variable"
        def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
        def SF_JWT_CRED_ID=env.SF_JWT_CRED_ID
        def SF_HOST = env.SF_HOST ?: "https://login.salesforce.com"
        def SF_HUB_ORG=env.SF_HUB_ORG
	    
	def toolbelt = tool 'toolbelt'
        
        echo "Check Out the Source Code"
        stage('Checkout'){
            checkout scm
        }
        
        echo "Wrap All Stages in a withCredentials Command"
   	withCredentials([file(credentialsId: SF_JWT_CRED_ID, variable: 'server_key_file')]) {
		
		stage ('Authentication'){
			rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${SF_CONSUMER_KEY} --username ${SF_HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SF_HOST}
		}
		
		if (rc != 0) { error 'hub org authorization failed' }

		println rc
        
	stage ('PackageValidation') {
	echo "entering package validation"
	}
            
     
    
    
    }
    
    
    catch(err){
        echo "build failed"
    }

}

