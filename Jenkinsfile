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
                rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid 3MVG9sh10GGnD4DvmSvN.HRuDc3svWt54HKaoNw25n1No6z3bG4VrQ6oEGvwGDrtb9MEquFMkpSeWQIXc.38e --username haripriya.hurrill@curious-unicorn-9n1vid.com --jwtkeyfile 3c127730-8496-420f-bcc8-2a6de8539f9b --setdefaultdevhubusername --instanceurl https://login.salesforce.com"
            }
            
            if (rc != 0) { error 'hub org authorization failed' }
            println rc
            
            // need to pull out assigned username
			if (isUnix()) {
				rmsg = sh returnStdout: true, script: "${toolbelt} force:mdapi:deploy -d ./package.xml -u haripriya.hurrill@curious-unicorn-9n1vid.com"
			}else{
			   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d ./package.xml -u haripriya.hurrill@curious-unicorn-9n1vid.com"
			}

            printf rmsg
            println('Hello from a Job DSL script!')
            println(rmsg)

            stage ('PackageValidation') {
            echo "entering package validation"
            }
        
        }
    
    
    catch(err){
        echo "build failed"
    }

    }
}
